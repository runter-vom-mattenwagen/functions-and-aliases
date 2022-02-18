# Functions And Aliases

> Small list of useful functions and aliases that fit perfectly in your .bashrc.

## Table of Contents
- [Functions](#functions)
- [Aliases](#aliases)

### Functions

#### Got [Todo.txt](http://todotxt.org)? Show tasks that refer only to current directory

```sh
td(){ todo.sh ${@-lsa} @${PWD##/*/}; }
```

#### Convert Markdown file to HTML and then display it

_Starts browser if configured as /etc/alternatives/x-www-browser, starts web server if not._
_Might need modifications depending on your environment_

_Usage: md2html <file.md>_

```sh
md2html ()
{
    pandoc $1 -f gfm -t html5 -s -o $1.html --metadata title="($1)";
    [[ -f $(which x-www-browser) ]] && x-www-browser $1.html || python3 -m http.server
}
```

#### Handy portscan without additional software

_Usage: pscan host.domain [port] - Scans single port or range from 1 to 1023_

```sh
pscan ()
{
    [[ -n $2 ]] && sq="$2 $2" || sq="1023"
    echo -en "\033[s"
    for p in $(seq $sq); do
        echo -en "\033[u$p"
        (  < /dev/tcp/$1/$p ) &> /dev/null && echo ": open"
    done
    echo -en "\033[u    "
}
```

#### Back to root of Git project from the abysses of subdirectories

_Iterates itself up to the first find of a directory named .git_

```sh
groot ()
{
    pushd . &> /dev/null
    until [[ -d .git ]] || [[ "$PWD" == "/" ]]; do
        cd ..
    done
    if [[ "$PWD" == "/" ]]; then
        echo "No Git until /"
        popd &> /dev/null
    fi
}
```
- - -

### Aliases

#### Opens up a host-related file for saving notes

```sh
alias n='vi "+normal G" ~/${HOSTNAME}_note.md'
```

#### Permission denied? ```sudo !!``` the smart way

_Usage: please - Runs last command with preceding sudo_

```sh
alias please='sudo $(fc -ln -1)'
```

#### Displays Git log as a coloful list

```sh
alias gl='git log --graph --pretty=format:"%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset" --abbrev-commit'
```  
