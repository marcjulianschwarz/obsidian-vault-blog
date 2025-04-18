---
blog-title: Command Line - Aliases and Functions
blog-published: 2023-07-05
blog-tags:
  - EN
  - TIL
  - Python
  - Misc
---

> **Edit** (17.12.2024): After more than a year of using Oh My Zsh and many custom aliases, my most used and favourite aliases are `go <marker-name>` (for going to a marked directory) and `b` (short for back + performing `cd ..`).  Oh My Zsh is awesome!

> **Edit** (20.07.2023): The open source framework [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)  makes it even easier to manage complex zsh configurations and there are tons of plugins that add useful aliases. 


Today I learned ([TIL](https://www.marc-julian.de/tags/TIL.html)) that you can define aliases and functions inside of most command line applications like bash or zsh shells. 
For zsh, the config file `.zshrc` is located directly in the user folder. You can edit it in any text editer (on some systems, you might need to open it as an admin or run your preferred command with `sudo`).

When you are done, run the following command to execute the file and reload all defined aliases and functions.

```shell
source .zshrc
```

Here are some of *my favorites*:

## Conda 

```shell
# Run to get bash autocomplete functions in zsh shell 
autoload bashcompinit
bashcompinit
autoload -Uz compinit
compinit

# List all conda environments
function envs() {
	conda info --env
}

# Activate environment with autocompletion
function aenv(){
    conda activate $1
}

function extract_env_names() {
    # Extracts the names of the environments from the output of `conda info --env`
    conda info --env | awk -F' ' '{print $1}' | tail -n +3
}

function _aenv() {
    # Get the current word being completed
    local cur=${COMP_WORDS[COMP_CWORD]}

    # Generate possible matches and store them in the COMPREPLY variable
    # -W expects a list of possible matches
    COMPREPLY=($(compgen -W "$(extract_env_names)" -- $cur))
}

# Register the completion function to be called for the aenv command
complete -F _aenv aenv

function denv(){
    conda deactivate
}

# Create a conda environment with a name and python version
function cenv(){
    conda create -n $1 python=$2
}

function renv(){
    conda remove -n $1 --all
}

# Create a python kernel from a conda environment
function envtokernel(){
    python -m ipykernel install --user --name $1 --display-name "Python ($1)"
}

# Create a conda environment with some sane default packages that I use a lot in data science projects 
function defaultenv(){
    cenv $1 $2
    conda activate $1
    pip install pandas numpy matplotlib seaborn scikit-learn notebook
    envtokernel $1
}
```

## Mark paths to directly jump to them from anywhere 

Inspired by the `mark` and `cdd` function in this [blog post](https://chris-said.io/2014/10/16/jumping-quickly-between-deep-directories/) by Chris Said, I added two more additional functions `opend` and `coded` to quickly open a folder in Finder or VSCode.

```shell
# Run to get bash autocomplete functions in zsh shell 
autoload bashcompinit
bashcompinit
autoload -Uz compinit
compinit

export MARKPATH="/Users/username/marks"

# jump to a marked path
function cdd() {
    # change directory to the marked path, if it does not exist, ignore the error and print "No such mark: $1" instead
    cd -P "$MARKPATH/$1" 2>/dev/null || echo "No such mark: $1"
}

function opend() {
    open "$MARKPATH/$1" 2>/dev/null || echo "No such mark: $1"
}

function coded() {
    code "$MARKPATH/$1" 2>/dev/null || echo "No such mark: $1"
}

# mark a path for quick access
function mark() {
    # create a folder to store marks
    mkdir -p "$MARKPATH"

    # create a symlink in the mark folder to the current directory
    ln -s "$(pwd)" "$MARKPATH/$1"
}

# delete a mark
function unmark() {
    # remove the symlink
    rm -i "$MARKPATH/$1"
}

# list all marks
function marks() {
    # list all symlinks in the mark folder with some formatting magic to make it look nicer
    \ls -l "$MARKPATH" | tail -n +2 | sed 's/  / /g' | cut -d' ' -f9- | awk -F ' -> ' '{printf "%-10s -> %s\n", $1, $2}'
}

# autocomplete marks
function _cdd() {
    # get the current word being completed
    local cur=${COMP_WORDS[COMP_CWORD]}
    # generate possible matches and store them in the COMPREPLY variable
    COMPREPLY=($(compgen -W "$(ls $MARKPATH)" -- $cur))
}

# register the completion function to be called for the cdd command
complete -F _cdd cdd
complete -F _cdd opend
complete -F _cdd coded
```

## Python 

```shell
function py(){
    python $1
}

function pyv(){
    python --version
}
```


## Streamlit 

```shell
function strun(){
    streamlit run $1
}
```

## Jupyter 

```shell
alias jnb="jupyter notebook"
```

## Git 

```shell
function gitc(){
    git commit -m "$1"
}

alias gits="git status"
alias gitp="git push"
alias gitpl="git pull"
alias gitb="git branch"
alias gitl="git log"
alias gitd="git diff"
```

## NPM 

```shell
alias ninit="npm init -y"
alias nstart="npm start"
alias nbuild="npm run build"
alias ntest="npm run test"
alias ninstall="npm install"
```

## ImageMagick

```shell
# d for do mogrify 
# I use this to automatically convert images for this blog 
function dmog(){
    mogrify -format jpg -geometry 1300x -quality $1 *.png
}
```

