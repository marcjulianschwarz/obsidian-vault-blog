---
blog-title: Text Generation using Markov Chains
blog-published: 2022-07-25
blog-tags:
  - Python
  - NLP
  - EN
  - Data Science
---

Let's create a simple text generation model using Markov chains. You can play with [this demo](https://marcjulianschwarz-markov-chain-text-genera-generate-text-m4bkd9.streamlitapp.com) to see what we're trying to build today.

## What is a Markov Chain?

A basic Markov chain consist of three main parts 
- a state space,
- a transition matrix
- and an initial distribution

We will go through the example of **Weather Forecast for the next X days** to get a better understanding of the different parts.

For a state space we could think of the following states:
- Rain 🌧️
- Sun ☀️
- Snow ❄️

> Of course, to model the real weather we might want to increase the size of the state space by a lot.

Now, we can model the weather by providing different probabilities to get from one state to another.

The probabilities (also known as **transition matrix**) could for example be 
- 0.2 for 🌧️  → ☀️ 
- 0.3 for 🌧️ → ❄️
- 0.5 for 🌧️ → 🌧️
- 0.3 for ❄️ → 🌧️
- 0.1 for ❄️ → ☀️
- 0.6 for ❄️ → ❄️
- 0.1 for ☀️ → ❄️
- 0.2 for ☀️ → 🌧️
- 0.7 for ☀️ → ☀️

Now let us do a 5 day weather forecast and on day one the sun is shining (this first state is also known as the **Initial Distribution**). We have to sample from our state space with the probabilities from above to determine the next state for a given current state.

**Day 1**
- Current State: ☀️
- Sample State: ☀️

**Day 2**
- Current State: ☀️
- Sample State: ☀️

**Day 3**
- Current State: ☀️
- Sample State: 🌧️

**Day 4**
- Current State: 🌧️
- Sample  State: 🌧️

**Day 5**
- Current State: 🌧️
- Sample State: ❄️

So, the weather forecast will look like this:

☀️ → ☀️ → 🌧️ → 🌧️ → ❄️ → ...


## Text Generation with Markov Chains 

We can apply the same concept to words of a given text. Each unique word represents a single state. A sentence is a sequence of states that were sampled from a state space. 

1. Create a state space
2. Compute probabilities for the transition matrix
3. Sample from the state space with the transition matrix

### 1. Create a State Space 

We will create the state space from a given source text. 

To do this, we read in the text as a string and split the text into individual words, often called **tokens**. Then we make sure that we have unique states by creating a set which is often called the **vocab**.

```python
with open("source_text.txt", "r") as f:
	source_text = f.read()

# Split the text into all tokens (words)
tokens = source_text.split(" ")

# This will make sure that we only have unique states
states = list(set(tokens))
```

### 2. Calculate Probabilites (Transition Matrix)

The transition matrix contains all possibilities of a state `i` followed by a state `j`. So we create a matrix with `len(states)` rows and `len(states)` columns:

```python
transition_matrix = np.zeroes((len(states), len(states)))
```

First, we count the number of occurrences for each state and each state transition (i.e. two words following each other).

```python
state_transition_counts = {}
state_counts = {}

for i, state in enumerate(states):
	if i < len(states) - 1:
		# Get the transition from current to next state
		state_transition = (state, states[i + 1])

		state_transition_counts[from_to] = state_transition_counts.get(state_transition, 0) + 1
		state_counts[state] = state_counts.get(state, 0) + 1
```

Then, for each state transition, we calculate its relative frequency and store this probability in the transition matrix at an index that we get by encoding each state transition with two integers (the two states index in the vocab list). 

```python
for state_transition, count in state_transition_counts.items():
	from_state, to_state = state_transition
	
	# Relative Frequency 
	probability = count / state_counts[from_state]
	
	matrix_row_index = states.index(from_state)
	matrix_col_index = states.index(to_state)
	
	transition_matrix[matrix_row_index, matrix_col_index] = probability
```

This is how a transition matrix could look like if you color all non-zero entries.
![Scatter Plot of a Transition Matrix](/images/transition_matrix.jpg)

### 3. Sample from the Transition Matrix 

Now, we are ready to sample words from the transition matrix. As we saw above, we need a *current* state to begin generating more states. 
A more sophisticated way than having a fixed first state is to provide an **initial distribution** that contains probabilities for each word to come first.

The initial distribution is a vector of length `len(states)`. With it we can force certain words to be more likely to start the Markov chain. In an extreme case we might want the generated text to start with a certain word (e.g.: *"The"*). We look for the index of *"The"* in our states and set that index to 1 in the initial distribution vector and all other indices to 0. 

```python
initial_distribution = np.zeroes((len(states))
initial_distribution[states.index("The")] = 1
```

We could also use a list of possible sentence beginnings, which should all start a sentence with equal probability.

```python
initial_distribution = np.zeroes((len(states))

sentence_beginnings = ["The", "This", "I", "You", "We", "That"]

for sentence_beginning in sentence_beginnings:
								 
	initial_distribution[states.index(sentence_beginning)] = 1 / len(sentence_beginnings)
```

To generate a text, we sample words from the list of states with a probability determined by the initial distribution and the transition matrix until we reach a maximum text length. If there is no initial distribution, we randomly choose a starting word. 

```python 
def generate_text(tokens: List[str], length: int, P_matrix: np.ndarray, P_init: np.ndarray=None) -> str:

	text = []

	if P_init is not None:
		current_token = np.random.choice(tokens, p=P_init)
	else:
		current_token = np.random.choice(tokens)

	text.append(current_token)

	for i in range(length):

		current_token = np.random.choice(tokens, p=P_matrix[tokens.index(current_token)])
		text.append(current_token)

	return " ".join(text)
```


## Try it 

Now have fun generating texts. Be sure to try different source texts and see how that affects the quality of the generated text. 

```python
generate_text(states, 100, transition_matrix)
```

You can also try out the [text generation web app](https://marcjulianschwarz-markov-chain-text-genera-generate-text-m4bkd9.streamlitapp.com/) I built and take a look at the [GitHub repo](https://github.com/marcjulianschwarz/markov-chain-text-generation) which contains the source code.



## Examples 

Some German examples.

```
Winchester, Spring Valley, das Anstauen des Colorado River und teuer, um diese Gärten der Wasserknappheit > Wir sind die Bevölkerung muss als in Hotelzimmern von Las Vegas, der Stadt, näher betrachtet werden mit etwa vier Prozent aus dem Haus kein wertvoller Baugrund verwendet, welches bis zu minimieren, muss das Licht auf die Wiederverwendung von Las Vegas. Um die Sicherung der Wasserversorgung
```

```
Glücksspielmetropole der Hotels können selbst schon durch diesen neuen wassersparenden Technologien wie Kakteen in der größte Bereich der vielen exotischen Pflanzen und hat sich als in sonnenbetriebene Kraftwerke investieren. Besonders die Zukunft der Bau des Wassers ist der Lebensraum geraubt. Auch die Uhr mit Trockenzeiten in keinster Weise optimal für die Behörde sogar ganz gestoppt werden. Ein bekannter botanischer Garten, ist 
```

```
individuellen Leistungsstand an die Höhe in Extremsportarten in den Rest des Selbstbewusstseins und unabhängig an positiven Gefühle, die er sich mit ihr umzugehen und hilft uns vor einem keine allgemeine Definition für sich relativ einfach nur das eigene Wohlempfinden zu gefährden, lässt sich lohnt dieses Risiko verbunden. Durch die Angst. Die Angst die Bremsen nicht vorhanden. 5 Fazit vorgestellt, in Hunderten
```