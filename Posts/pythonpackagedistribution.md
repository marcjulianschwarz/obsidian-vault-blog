---
blog-title: Distributing a Python Package
blog-published: 2022-02-11
blog-tags:
  - EN
  - Python
blog-archived: true
---
> **Notice:** This blog post is outdated and there are better ways to package and publish Python packages.

Yesterday I published my Python package [watchlib](https://github.com/marcjulianschwarz/watchlib) to PyPi, the *Python Package Index*. In this post I will show you how I did it and add some tips, tricks and resources that helped me a lot in the process.

## Package structure

To make a package ready for distribution you will have to create a folder that will look something like this:

```
some_folder/
├── LICENSE
├── pyproject.toml
├── README.md
├── setup.cfg
├── src/
│   └── your_package/
│       ├── __init__.py
│       └── submodule_of_your_package.py
└── tests/
```

If you are using GitHub to manage your package you will know what a README.md and LICENSE file is. But, if you need some help with choosing an appropriate license or creating an appealing readme, here are some resources that you can check out:
- [choosealicense.com](https://choosealicense.com)
- [Writing an Formatting on GitHub](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [GitHub flavored markdown](https://github.github.com/gfm/)

The `src` folder will contain all your sources and the top level folder in there will determine the name of your package. Adding an empty `__init__.py` file ensures that you can import the folder later on.
## Build configuration - pyproject.toml 

In the `pyproject.toml` file you will have to enter configuration details that the build process will use. To build my project I was using [setuptools](https://setuptools.pypa.io/en/latest/) which greatly simplifies the building process. If you want to use setuptools too, make sure to require it and define the build-backend (in the pyproject.toml file) like this:

```
[build-system]
requires = [
 "setuptools>=42",
 "wheel"
]
build-backend = "setuptools.build_meta"
```

## Metadata - setup.cfg 

The setup.cfg file is the configuration file for setuptools itself. You define all metadata of your package here. The resource that helped me the most was the [userguide from setuptools](https://setuptools.pypa.io/en/latest/userguide/declarative_config.html). It's great.

For some inspiration, this is how my setup.cfg file looks like:

```
[metadata]
name = watchlib
version = 0.0.1a1
author = Marc Julian Schwarz
author_email = my@email.de
description = watchlib is a [[Python]] package providing tools for loading, visualizing and analyzing Apple Watch health data.
long_description = file: README.md
long_description_content_type = text/markdown
url = https://github.com/marcjulianschwarz/watchlib

project_urls =
	Bug Tracker = https://github.com/marcjulianschwarz/watchlib/issues
	Docs = https://github.com/marcjulianschwarz/watchlib/wiki

classifiers =
	Programming Language :: Python :: 3
	License :: OSI Approved :: MIT License
	Operating System :: OS Independent

[options]
package_dir =
	= src
packages = find:
python_requires = >=3.6
install_requires =
	pandas
	numpy
	matplotlib
	seaborn
	annoy
	sklearn

[options.packages.find]

where = src

```

As you can see, I used the `install_requires` field to list all dependencies of my package. When someone installs the package via PyPi it will automatically install these dependencies as well.
One last note on the version field. [PEP 440](https://www.python.org/dev/peps/pep-0440/) goes into great detail on different version schemes, what's allowed and what not and lists some interesting version timeline examples. 

## Building the package 

Now comes the exciting part. To build the package, navigate to the folder where you created the `pyproject.toml` file and run this command:

```
python3 -m build
```

When building has finished a new folder, called `dist`, should appear on the same level as the `src` folder. It contains a source archive and a built distribution.

## Uploading distribution files with Twine

Before uploading the files you will have to register an account on [PyPi](https://pypi.org). 

>**Important side note!** I would highly recommend to first register on [Test PyPi](https://test.pypi.org) which is a second Python package index for testing. Everything works the same but you can treat it as a playground and experiment a bit. 

When you have an account you will need to create an [API token](https://test.pypi.org/account/login/?next=%2Fmanage%2Faccount%2F#api-tokens) and store it somewhere safe.

Now that everything is setup you can start uploading your package using twine like this:

```
twine upload dist/*
```

To upload to [Test PyPi](https://test.pypi.org) use this command instead:

```
python3 -m twine upload --repository testpypi dist/*
```

## And that's it! You distributed a Python package.

To install your package, run the familiar pip command:

```
pip install your_package
```

If you used [Test PyPi](https://test.pypi.org) you can install your package like this:

```
python3 -m pip install --index-url https://test.pypi.org/simple/ --no-deps your_package
```
