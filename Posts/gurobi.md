---
blog-title: Gurobi - Solving Mixed Integer Problems with Python
blog-published: 2022-12-31
blog-tags:
  - Python
  - Math
  - Data Science
blog-subtitle: Capacitated and Uncapacitated Lot-Sizing Problems
---

Solving a Mixed Integer Programming (MIP) Problem with Gurobi requires the [Gurobi Optimizer](https://www.gurobi.com/products/gurobi-optimizer/) and the Python package [GurobiPy](https://support.gurobi.com/hc/en-us/articles/360044290292-How-do-I-install-Gurobi-for-Python-) to be installed on your system.

The GurobiPy package can be installed with pip or conda:

```bash
python -m pip install gurobipy
```

```bash
conda install -c gurobi gurobi
```

## Importing package 

When installed, gurobipy can be imported like this:

```python
import gurobipy as gp
from gurobipy import GRB
```

## Initialize Model 

The next step is to initialize a Gurobi model. For this example problem, setting a constant with the name capacitated to `False` will be necessary too. This variable can be changed depending on whether you want to have production capacity constraints (LS-C) or not (LS-U).

```python
m = gp.Model("ls")

capacitated = False
```

## Constants 

The situation for the example problem is the following:

A production facility wants to make a plan for the next 8 months. They want to know exactly how many products have to be produced each month to fulfill the demand in that month and reduce the overall costs which consist of

- Production costs for one product: 100€ 
- Preparing the machines at the beginning of a month (only if the machines are being used in that month):  5000€
- Storing one product for a month:  5€

These are all constants that won't change during the optimization of the model. That's why they don't have to be added to the model.

```python
# Time (e.g. months)
n = 8
t = range(0, n)
  
# Costs for production of one product for each month
p = [100 for i in t]

# Costs for storing a product for each month
h = [5 for i in t]  

# Costs for armoring the machines for each month (has to be done at most once for each month)
q = [5000 for i in t]

# Demand for products for each month
d = [400, 400, 800, 800, 1200, 1200, 1200, 1200]
```

## Add Variables

To describe the objective function and constraints of the model, variables with specified types have to be added to the model.

The number of products being produced each month (`x`) will have to be an integer variable as the facility can't produce fractions of a product. The same constraint holds for the storage of products (`s`) where only finished products can be stored.
The storage amount will have one extra entry in comparison to the other variables as there will be an initial stock from the month before.

The variable `y` is binary as it controls whether the machines have to be prepared (1) or not (0).

The only difference between the Uncapacitated and Capacitated Lot-Sizing problem lies in the definition of the capacity constraining variable `M`. 
For the uncapacitated case, the variable will be unbounded and will therefore always be big enough to produce any amount of products. In the capacitated case it will consist of a list of capacities.
In the code example below, the capacities are selected in a way to minimize the preparation costs of the machines.

```python
# Production Amount for each month
x = m.addVars(n, name="x", vtype=GRB.INTEGER)

# Storage Amount for each month
s = m.addVars(n + 1, name="s", vtype=GRB.INTEGER)

# Production Preparation necessary for each month (0 or 1)
y = m.addVars(n, name="y", vtype=GRB.BINARY)

# Production Capacity for each month (in this case unbounded -> LS-U)
if capacitated:
	M = [7000, 0, 0, 0, 0, 0, 0, 0]
else:
	M = m.addVars(n, name="M", vtype=GRB.INTEGER)
```

## Objective Function

As described above, the facility wants to minimize the overall costs. They consist of the sum of costs for producing products, preparing machines, and storing products.
Thus the objective function has to look like this:

```python
m.setObjective(gp.quicksum(p[i] * x[i] + q[i] * y[i] + h[i] * s[i+1] for i in t), GRB.MINIMIZE)
```

## (Linear) Constraints

Last but not least all constraints of the model have to be defined.

The first two constraints make sure that the demand will be fulfilled and that the excess will be stored for the next month.

Constraints three and four will control the initial and final stock.

Constraint five adds capacity constraints to the model. As stated before, `M` will either be unbounded or have specific capacity restrictions.

The last three positivity constraints make sure that no negative amounts of products can be produced or stored.

```python
# Stored products from the previous month plus the number of products produced in the current
# month must fulfill the demand while the rest of the products must be stored for the next month
m.addConstrs((s[i-1] + x[i-1] == d[i-1] + s[i] for i in range(1, n+1)), "c1")
m.addConstr((s[0] + x[0] == d[0] + s[1]), "c2")

# The number of products stored in the first month must be equal to 200 (Initial stock)
m.addConstr(s[0] == 200, "c3")

# The number of products stored in the last month must be equal to 0 (Final stock)
m.addConstr(s[n] == 0, "c4")

# If products are being produced in the current month the machines must be prepared
m.addConstrs((x[i] <= M[i]*y[i] for i in t), "c5")

# There cant be a negative number of products stored in the warehouse or produced
m.addConstrs((x[i] >= 0 for i in t), "c6")
m.addConstrs((s[i] >= 0 for i in t), "c7")
m.addConstrs((M[i] >= 0 for i in t), "c8");
```


## Mathematical Model 

The resulting mathematical model would look like this:

![Mathematical LP model](/images/LS_U_Problem_Model.svg)

## Optimizing 

The function call

```python
m.optimize()
```

does everything we need to solve this optimization problem.

Gurobi will use the Branch-and-Bound algorithm as the model only consists of linear constraints and a linear objective function, with (mixed) integer variables (IP).

## Evaluation 

These are the results for the given problem:     

```
Gurobi Optimizer version 9.5.2 build v9.5.2rc0 (mac64[rosetta2])
Thread count: 10 physical cores, 10 logical processors, using up to 10 threads
Optimize a model with 35 rows, 33 columns and 53 nonzeros
Model fingerprint: 0xa7ef1cf7
Model has 8 quadratic constraints
Variable types: 0 continuous, 33 integer (8 binary)
Coefficient statistics:
  Matrix range     [1e+00, 1e+00]
  QMatrix range    [1e+00, 1e+00]
  QLMatrix range   [1e+00, 1e+00]
  Objective range  [5e+00, 5e+03]
  Bounds range     [1e+00, 1e+00]
  RHS range        [2e+02, 1e+03]
Presolve removed 29 rows and 4 columns
Presolve time: 0.00s
Presolved: 30 rows, 53 columns, 74 nonzeros
Presolved model has 16 SOS constraint(s)
Variable types: 0 continuous, 53 integer (16 binary)
Found heuristic solution: objective 859000.00000
Found heuristic solution: objective 830000.00000
Found heuristic solution: objective 822000.00000

Root relaxation: objective 7.170501e+05, 13 iterations, 0.00 seconds (0.00 work units)

    Nodes    |    Current Node    |     Objective Bounds      |     Work
 Expl Unexpl |  Obj  Depth IntInf | Incumbent    BestBd   Gap | It/Node Time

     0     0 717050.070    0    6 822000.000 717050.070  12.8%     -    0s
H    0     0                    737000.00000 717050.070  2.71%     -    0s
H    0     0                    736000.00000 727633.403  1.14%     -    0s
     0     0 736000.000    0    5 736000.000 736000.000  0.00%     -    0s

Cutting planes:
  Implied bound: 7
  Flow cover: 1

Explored 1 nodes (20 simplex iterations) in 0.01 seconds (0.00 work units)
Thread count was 10 (of 10 available processors)

Solution count 5: 736000 737000 822000 ... 859000

Optimal solution found (tolerance 1.00e-04)
Best objective 7.360000000000e+05, best bound 7.360000000000e+05, gap 0.0000%
```

As one can see the optimal solution is equal to 736,000€ of overall costs.

Printing out the variables reveals how many products have to be produced in each month to achieve these minimal costs.

```python
for v in m.getVars():

	if v.varName.startswith("x"):
		print("Produce {} products in month {}".format(v.x, v.varName[1:]))
	
print("--------------------------------")
print("Total cost: {}".format(m.objVal))
```

Output:

```
Produce 600.0 products in month [0] 
Produce 0.0 products in month [1] 
Produce 1600.0 products in month [2] 
Produce 0.0 products in month [3] 
Produce 1200.0 products in month [4] 
Produce 1200.0 products in month [5] 
Produce 1200.0 products in month [6] 
Produce 1200.0 products in month [7] 
-------------------------------- 
Total cost: 736000.0
```

