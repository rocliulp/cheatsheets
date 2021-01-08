# https://devhints.io/bash
## Common
### Introduction

This is a quick reference to getting started with Bash scripting.

- [Learn bash in y minutes](https://learnxinyminutes.com/docs/bash/)*(learnxinyminutes.com)*
- [Bash Guide](http://mywiki.wooledge.org/BashGuide)*(mywiki.wooledge.org)*

### Conditional execution

```bash
git commit && git push
git commit || echo "Commit failed"
```

### Strict mode

```bash
set -euo pipefail
IFS=$'\n\t'
```

See: [Unofficial bash strict mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/)

### Example

```bash
#!/usr/bin/env bash

NAME="John"
echo "Hello $NAME!"
```

### String quotes

```bash
NAME="John"
echo "Hi $NAME"  #=> Hi John
echo 'Hi $NAME'  #=> Hi $NAME
```

### Functions

```bash
get_name() {
  echo "John"
}

echo "You are $(get_name)"
```

See: [Functions](https://devhints.io/bash#functions)

### Brace expansion

```bash
echo {A,B}.js
```

| `{A,B}`    | Same as `A B`       |
| ---------- | ------------------- |
| `{A,B}.js` | Same as `A.js B.js` |
| `{1..5}`   | Same as `1 2 3 4 5` |

See: [Brace expansion](http://wiki.bash-hackers.org/syntax/expansion/brace)

### Variables

```bash
NAME="John"
echo $NAME
echo "$NAME"
echo "${NAME}!"
```

### Shell execution

```bash
echo "I'm in $(pwd)"
echo "I'm in `pwd`"
# Same
```

See [Command substitution](http://wiki.bash-hackers.org/syntax/expansion/cmdsubst)

### Conditionals

```bash
if [[ -z "$string" ]]; then
  echo "String is empty"
elif [[ -n "$string" ]]; then
  echo "String is not empty"
fi
```

See: [Conditionals](https://devhints.io/bash#conditionals)

## Parameter expansions

### Basics

```bash
name="John"
echo ${name}
echo ${name/J/j}    #=> "john" (substitution)
echo ${name:0:2}    #=> "Jo" (slicing)
echo ${name::2}     #=> "Jo" (slicing)
echo ${name::-1}    #=> "Joh" (slicing)
echo ${name:(-1)}   #=> "n" (slicing from right)
echo ${name:(-2):1} #=> "h" (slicing from right)
echo ${food:-Cake}  #=> $food or "Cake"
length=2
echo ${name:0:length}  #=> "Jo"
```

See: [Parameter expansion](http://wiki.bash-hackers.org/syntax/pe)

```bash
STR="/path/to/foo.cpp"
echo ${STR%.cpp}    # /path/to/foo
echo ${STR%.cpp}.o  # /path/to/foo.o
echo ${STR%/*}      # /path/to

echo ${STR##*.}     # cpp (extension)
echo ${STR##*/}     # foo.cpp (basepath)

echo ${STR#*/}      # path/to/foo.cpp
echo ${STR##*/}     # foo.cpp

echo ${STR/foo/bar} # /path/to/bar.cpp
STR="Hello world"
echo ${STR:6:5}   # "world"
echo ${STR: -5:5}  # "world"
SRC="/path/to/foo.cpp"
BASE=${SRC##*/}   #=> "foo.cpp" (basepath)
DIR=${SRC%$BASE}  #=> "/path/to/" (dirpath)
```

### Substitution

| `${FOO%suffix}`   | Remove suffix       |
| ----------------- | ------------------- |
| `${FOO#prefix}`   | Remove prefix       |
| `${FOO%%suffix}`  | Remove long suffix  |
| `${FOO##prefix}`  | Remove long prefix  |
| `${FOO/from/to}`  | Replace first match |
| `${FOO//from/to}` | Replace all         |
| `${FOO/%from/to}` | Replace suffix      |
| `${FOO/#from/to}` | Replace prefix      |

### Length

| `${#FOO}` | Length of `$FOO` |
| --------- | ---------------- |
|           |                  |

### Default values

| `${FOO:-val}`     | `$FOO`, or `val` if unset (or null)                      |
| ----------------- | -------------------------------------------------------- |
| `${FOO:=val}`     | Set `$FOO` to `val` if unset (or null)                   |
| `${FOO:+val}`     | `val` if `$FOO` is set (and not null)                    |
| `${FOO:?message}` | Show error message and exit if `$FOO` is unset (or null) |

Omitting the `:` removes the (non)nullity checks, e.g. `${FOO-val}` expands to `val` if unset otherwise `$FOO`.

### Comments

```bash
# Single line comment
: '
This is a
multi line
comment
'
```

### Substrings

| `${FOO:0:3}`    | Substring *(position, length)* |
| --------------- | ------------------------------ |
| `${FOO:(-3):3}` | Substring from the right       |

### Manipulation

```bash
STR="HELLO WORLD!"
echo ${STR,}   #=> "hELLO WORLD!" (lowercase 1st letter)
echo ${STR,,}  #=> "hello world!" (all lowercase)

STR="hello world!"
echo ${STR^}   #=> "Hello world!" (uppercase 1st letter)
echo ${STR^^}  #=> "HELLO WORLD!" (all uppercase)
```

## Functions

### Defining functions

```bash
myfunc() {
    echo "hello $1"
}
# Same as above (alternate syntax)
function myfunc() {
    echo "hello $1"
}
myfunc "John"
```

### Returning values

```bash
myfunc() {
    local myresult='some value'
    echo $myresult
}
result="$(myfunc)"
```

### Arguments

| `$#` | Number of arguments                   |
| ---- | ------------------------------------- |
| `$*` | All arguments                         |
| `$@` | All arguments, starting from first    |
| `$1` | First argument                        |
| `$_` | Last argument of the previous command |

See [Special parameters](http://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables).

### Raising errors

```bash
myfunc() {
  return 1
}
if myfunc; then
  echo "success"
else
  echo "failure"
fi
```

## Conditionals

### Conditions

Note that `[[` is actually a command/program that returns either `0` (true) or `1` (false). Any program that obeys the same logic (like all base utils, such as `grep(1)` or `ping(1)`) can be used as condition, see examples.

| `[[ -z STRING ]]`        | Empty string          |
| ------------------------ | --------------------- |
| `[[ -n STRING ]]`        | Not empty string      |
| `[[ STRING == STRING ]]` | Equal                 |
| `[[ STRING != STRING ]]` | Not Equal             |
| `[[ NUM -eq NUM ]]`      | Equal                 |
| `[[ NUM -ne NUM ]]`      | Not equal             |
| `[[ NUM -lt NUM ]]`      | Less than             |
| `[[ NUM -le NUM ]]`      | Less than or equal    |
| `[[ NUM -gt NUM ]]`      | Greater than          |
| `[[ NUM -ge NUM ]]`      | Greater than or equal |
| `[[ STRING =~ STRING ]]` | Regexp                |
| `(( NUM < NUM ))`        | Numeric conditions    |

#### More conditions

| `[[ -o noclobber ]]` | If OPTIONNAME is enabled |
| -------------------- | ------------------------ |
| `[[ ! EXPR ]]`       | Not                      |
| `[[ X && Y ]]`       | And                      |
| `[[ X || Y ]]`       | Or                       |

### File conditions

| `[[ -e FILE ]]`         | Exists                  |
| ----------------------- | ----------------------- |
| `[[ -r FILE ]]`         | Readable                |
| `[[ -h FILE ]]`         | Symlink                 |
| `[[ -d FILE ]]`         | Directory               |
| `[[ -w FILE ]]`         | Writable                |
| `[[ -s FILE ]]`         | Size is > 0 bytes       |
| `[[ -f FILE ]]`         | File                    |
| `[[ -x FILE ]]`         | Executable              |
| `[[ FILE1 -nt FILE2 ]]` | 1 is more recent than 2 |
| `[[ FILE1 -ot FILE2 ]]` | 2 is more recent than 1 |
| `[[ FILE1 -ef FILE2 ]]` | Same files              |

### Example

```bash
# String
if [[ -z "$string" ]]; then
  echo "String is empty"
elif [[ -n "$string" ]]; then
  echo "String is not empty"
else
  echo "This never happens"
fi
# Combinations
if [[ X && Y ]]; then
  ...
fi
# Equal
if [[ "$A" == "$B" ]]
# Regex
if [[ "A" =~ . ]]
if (( $a < $b )); then
   echo "$a is smaller than $b"
fi
if [[ -e "file.txt" ]]; then
  echo "file exists"
fi
```

## Arrays

### Defining arrays

```bash
Fruits=('Apple' 'Banana' 'Orange')
Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"
```

### Operations

```bash
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits+=('Watermelon')                  # Also Push
Fruits=( ${Fruits[@]/Ap*/} )            # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file
```

### Working with arrays

```bash
echo ${Fruits[0]}           # Element #0
echo ${Fruits[-1]}          # Last element
echo ${Fruits[@]}           # All elements, space-separated
echo ${#Fruits[@]}          # Number of elements
echo ${#Fruits}             # String length of the 1st element
echo ${#Fruits[3]}          # String length of the Nth element
echo ${Fruits[@]:3:2}       # Range (from position 3, length 2)
echo ${!Fruits[@]}          # Keys of all elements, space-separated
```

### Iteration

```bash
for i in "${arrayName[@]}"; do
  echo $i
done
```

## Dictionaries

### Defining

```bash
declare -A sounds
sounds[dog]="bark"
sounds[cow]="moo"
sounds[bird]="tweet"
sounds[wolf]="howl"
```

Declares `sound` as a Dictionary object (aka associative array).

### Working with dictionaries

```bash
echo ${sounds[dog]} # Dog's sound
echo ${sounds[@]}   # All values
echo ${!sounds[@]}  # All keys
echo ${#sounds[@]}  # Number of elements
unset sounds[dog]   # Delete dog
```

### Iteration

#### Iterate over values

```bash
for val in "${sounds[@]}"; do
  echo $val
done
```

#### Iterate over keys

```bash
for key in "${!sounds[@]}"; do
  echo $key
done
```

## Options

### Options

```bash
set -o noclobber  # Avoid overlay files (echo "hi" > foo)
set -o errexit    # Used to exit upon error, avoiding cascading errors
set -o pipefail   # Unveils hidden failures
set -o nounset    # Exposes unset variables
```

### Glob options

```bash
shopt -s nullglob    # Non-matching globs are removed  ('*.foo' => '')
shopt -s failglob    # Non-matching globs throw errors
shopt -s nocaseglob  # Case insensitive globs
shopt -s dotglob     # Wildcards match dotfiles ("*.sh" => ".foo.sh")
shopt -s globstar    # Allow ** for recursive matches ('lib/**/*.rb' => 'lib/a/b/c.rb')
```

Set `GLOBIGNORE` as a colon-separated list of patterns to be removed from glob matches.

## History

### Commands

| `history`             | Show history                              |
| --------------------- | ----------------------------------------- |
| `shopt -s histverify` | Don’t execute expanded result immediately |

### Operations

| `!!`                 | Execute last command again                                   |
| -------------------- | ------------------------------------------------------------ |
| `!!:s/<FROM>/<TO>/`  | Replace first occurrence of `<FROM>` to `<TO>` in most recent command |
| `!!:gs/<FROM>/<TO>/` | Replace all occurrences of `<FROM>` to `<TO>` in most recent command |
| `!$:t`               | Expand only basename from last parameter of most recent command |
| `!$:h`               | Expand only directory from last parameter of most recent command |

`!!` and `!$` can be replaced with any valid expansion.

### Expansions

| `!$`         | Expand last parameter of most recent command         |
| ------------ | ---------------------------------------------------- |
| `!*`         | Expand all parameters of most recent command         |
| `!-n`        | Expand `n`th most recent command                     |
| `!n`         | Expand `n`th command in history                      |
| `!<command>` | Expand most recent invocation of command `<command>` |

### Slices

| `!!:n`   | Expand only `n`th token from most recent command (command is `0`; first argument is `1`) |
| -------- | ------------------------------------------------------------ |
| `!^`     | Expand first argument from most recent command               |
| `!$`     | Expand last token from most recent command                   |
| `!!:n-m` | Expand range of tokens from most recent command              |
| `!!:n-$` | Expand `n`th token to last from most recent command          |

`!!` can be replaced with any valid expansion i.e. `!cat`, `!-2`, `!42`, etc.

## Miscellaneous

### Numeric calculations

```bash
$((a + 200))      # Add 200 to $a
$(($RANDOM%200))  # Random number 0..199
```

### Inspecting commands

```bash
command -V cd
#=> "cd is a function/alias/whatever"
```

### Trap errors

```bash
trap 'echo Error at about $LINENO' ERR
```

or

```bash
traperr() {
  echo "ERROR: ${BASH_SOURCE[1]} at about ${BASH_LINENO[0]}"
}

set -o errtrace
trap traperr ERR
```

### Source relative

```bash
source "${0%/*}/../share/foo.sh"
```

### Directory of script

```bash
DIR="${0%/*}"
```

### Getting options

```bash
while [[ "$1" =~ ^- && ! "$1" == "--" ]]; do case $1 in
  -V | --version )
    echo $version
    exit
    ;;
  -s | --string )
    shift; string=$1
    ;;
  -f | --flag )
    flag=1
    ;;
esac; shift; done
if [[ "$1" == '--' ]]; then shift; fi
```

### Special variables

| `$?` | Exit status of last task     |
| ---- | ---------------------------- |
| `$!` | PID of last background task  |
| `$$` | PID of shell                 |
| `$0` | Filename of the shell script |

See [Special parameters](http://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables).

### Check for command’s result

```bash
if ping -c 1 google.com; then
  echo "It appears you have a working internet connection"
fi
```

### Subshells

```bash
(cd somedir; echo "I'm now in $PWD")
pwd # still in first directory
```

### Redirection

```bash
python hello.py > output.txt   # stdout to (file)
python hello.py >> output.txt  # stdout to (file), append
python hello.py 2> error.log   # stderr to (file)
python hello.py 2>&1           # stderr to stdout
python hello.py 2>/dev/null    # stderr to (null)
python hello.py &>/dev/null    # stdout and stderr to (null)
python hello.py < foo.txt      # feed foo.txt to stdin for python
```

### Case/switch

```bash
case "$1" in
  start | up)
    vagrant up
    ;;

  *)
    echo "Usage: $0 {start|stop|ssh}"
    ;;
esac
```

### printf

```bash
printf "Hello %s, I'm %s" Sven Olga
#=> "Hello Sven, I'm Olga

printf "1 + 1 = %d" 2
#=> "1 + 1 = 2"

printf "This is how you print a float: %f" 2
#=> "This is how you print a float: 2.000000"
```

### Heredoc

```sh
cat <<END
hello world
END
```

### Reading input

```bash
echo -n "Proceed? [y/n]: "
read ans
echo $ans
read -n 1 ans    # Just one character
```

### Go to previous directory

```bash
pwd # /home/user/foo
cd bar/
pwd # /home/user/foo/bar
cd -
pwd # /home/user/foo
```

### Grep check

```bash
if grep -q 'foo' ~/.bash_history; then
  echo "You appear to have typed 'foo' in the past"
fi
```

## Also see

- [Bash-hackers wiki](http://wiki.bash-hackers.org/) *(bash-hackers.org)*
- [Shell vars](http://wiki.bash-hackers.org/syntax/shellvars) *(bash-hackers.org)*
- [Learn bash in y minutes](https://learnxinyminutes.com/docs/bash/) *(learnxinyminutes.com)*
- [Bash Guide](http://mywiki.wooledge.org/BashGuide) *(mywiki.wooledge.org)*
- [ShellCheck](https://www.shellcheck.net/) *(shellcheck.net)*

# bash-hackers.org

****Compound commands\****

| **[Compound commands overview](https://wiki.bash-hackers.org/syntax/ccmd/intro)** |                                                              |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| Grouping                                                     |                                                              |
| `{ …; }`                                                     | [command grouping](https://wiki.bash-hackers.org/syntax/ccmd/grouping_plain) |
| `( … )`                                                      | [command grouping in a subshell](https://wiki.bash-hackers.org/syntax/ccmd/grouping_subshell) |
| Conditionals                                                 |                                                              |
| `[[ ... ]]`                                                  | [conditional expression](https://wiki.bash-hackers.org/syntax/ccmd/conditional_expression) |
| `if …; then …; fi`                                           | [conditional branching](https://wiki.bash-hackers.org/syntax/ccmd/if_clause) |
| `case … esac`                                                | [pattern-based branching](https://wiki.bash-hackers.org/syntax/ccmd/case) |
| Loops                                                        |                                                              |
| `for word in …; do …; done`                                  | [classic for-loop](https://wiki.bash-hackers.org/syntax/ccmd/classic_for) |
| `for ((x=1; x<=10; x++)); do ...; done`                      | [C-style for-loop](https://wiki.bash-hackers.org/syntax/ccmd/c_for) |
| `while …; do …; done`                                        | [while loop](https://wiki.bash-hackers.org/syntax/ccmd/while_loop) |
| `until …; do …; done`                                        | [until loop](https://wiki.bash-hackers.org/syntax/ccmd/until_loop) |
| Misc                                                         |                                                              |
| `(( ... ))`                                                  | [arithmetic evaluation](https://wiki.bash-hackers.org/syntax/ccmd/arithmetic_eval) |
| `select word in …; do …; done`                               | [user selections](https://wiki.bash-hackers.org/syntax/ccmd/user_select) |

****Expansions and substitutions\****

| **[Introduction to expansions and substitutions](https://wiki.bash-hackers.org/syntax/expansion/intro)** |                                                              |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| `{A,B,C} {A..C}`                                             | [Brace expansion](https://wiki.bash-hackers.org/syntax/expansion/brace) |
| `~/ ~root/`                                                  | [Tilde expansion](https://wiki.bash-hackers.org/syntax/expansion/tilde) |
| `$FOO ${BAR%.mp3}`                                           | [Parameter expansion](https://wiki.bash-hackers.org/syntax/pe) |
| ``command` $(command)`                                       | [Command substitution](https://wiki.bash-hackers.org/syntax/expansion/cmdsubst) |
| `<(command) >(command)`                                      | [Process substitution](https://wiki.bash-hackers.org/syntax/expansion/proc_subst) |
| `$((1 + 2 + 3)) $[4 + 5 + 6]`                                | [Arithmetic expansion](https://wiki.bash-hackers.org/syntax/expansion/arith) |
| `Hello <---> Word!`                                          | [Word splitting](https://wiki.bash-hackers.org/syntax/expansion/wordsplit) |
| `/data/*-av/*.mp?`                                           |                                                              |

## Builtin Commands

| Declaration commands Commands that set and query attributes/types, and manipulate simple datastructures. |                             Alt                              |     Type     |                 |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------: | --------------- |
| [declare](https://wiki.bash-hackers.org/commands/builtin/declare) | Display or set shell variables or functions along with attributes. |  `typeset`   | builtin         |
| [export](https://wiki.bash-hackers.org/commands/builtin/export) | Display or set shell variables, also giving them the export attribute. | `typeset -x` | special builtin |
| [eval](https://wiki.bash-hackers.org/commands/builtin/eval)  |              Evaluate arguments as shell code.               |      -       | special builtin |
| [local](https://wiki.bash-hackers.org/commands/builtin/local) |      Declare variables as having function local scope.       |      -       | builtin         |
| [readonly](https://wiki.bash-hackers.org/commands/builtin/readonly) |          Mark variables or functions as read-only.           | `typeset -r` | special builtin |
| [unset](https://wiki.bash-hackers.org/commands/builtin/unset) |                Unset variables and functions.                |      -       | special builtin |
| [shift](https://wiki.bash-hackers.org/commands/builtin/shift) |                 Shift positional parameters                  |      -       | special builtin |

| I/O Commands for reading/parsing input, or producing/formatting output of standard streams. |                             Alt                              |    Type     |         |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :---------: | ------- |
| [coproc](https://wiki.bash-hackers.org/syntax/keywords/coproc) | Co-processes: Run a command in the background with pipes for reading / writing its standard streams. |      -      | keyword |
| [echo](https://wiki.bash-hackers.org/commands/builtin/echo)  |                Create output from arguments.                 |      -      | builtin |
| [mapfile](https://wiki.bash-hackers.org/commands/builtin/mapfile) |              Read lines of input into an array.              | `readarray` | builtin |
| [printf](https://wiki.bash-hackers.org/commands/builtin/printf) |                      "advanced `echo`."                      |      -      | builtin |
| [read](https://wiki.bash-hackers.org/commands/builtin/read)  | Read input into variables or arrays, or split strings into fields using delimiters. |      -      | builtin |

| Configuration and Debugging Commands that modify shell behavior, change special options, assist in debugging. |                             Alt                              | Type |                 |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :--: | --------------- |
| [caller](https://wiki.bash-hackers.org/commands/builtin/caller) |               Identify/print execution frames.               |  -   | builtin         |
|  [set](https://wiki.bash-hackers.org/commands/builtin/set)   | Set the positional parameters and/or set options that affect shell behaviour. |  -   | special builtin |
| [shopt](https://wiki.bash-hackers.org/commands/builtin/shopt) |          set/get some bash-specific shell options.           |  -   | builtin         |

| Control flow and data processing Commands that operate on data and/or affect control flow. |                         Alt                          |  Type  |                 |
| :----------------------------------------------------------: | :--------------------------------------------------: | :----: | --------------- |
| [colon](https://wiki.bash-hackers.org/commands/builtin/true) |                 "true" null command.                 |  true  | special builtin |
| [dot](https://wiki.bash-hackers.org/commands/builtin/source) |                Source external files.                | source | special builtin |
| [false](https://wiki.bash-hackers.org/commands/builtin/false) |                Fail at doing nothing.                |   -    | builtin         |
| [continue / break](https://wiki.bash-hackers.org/commands/builtin/continuebreak) |         continue with or break out of loops.         |   -    | special builtin |
|  [let](https://wiki.bash-hackers.org/commands/builtin/let)   |        Arithmetic evaluation simple command.         |   -    | builtin         |
| [return](https://wiki.bash-hackers.org/commands/builtin/return) | Return from a function with a specified exit status. |   -    | special builtin |
|   [[](https://wiki.bash-hackers.org/commands/classictest)    |          The classic `test` simple command.          |  test  | builtin         |

| Process and Job control Commands related to jobs, signals, process groups, subshells. |                          Alt                           | Type |                 |
| :----------------------------------------------------------: | :----------------------------------------------------: | :--: | --------------- |
| [exec](https://wiki.bash-hackers.org/commands/builtin/exec)  | Replace the current shell process or set redirections. |  -   | special builtin |
| [exit](https://wiki.bash-hackers.org/commands/builtin/exit)  |                    Exit the shell.                     |  -   | special builtin |
| [kill](https://wiki.bash-hackers.org/commands/builtin/kill)  |         Send a signal to specified process(es)         |  -   | builtin         |
| [trap](https://wiki.bash-hackers.org/commands/builtin/trap)  |  Set signal handlers or output the current handlers.   |  -   | special builtin |
| [times](https://wiki.bash-hackers.org/commands/builtin/times) |                 Display process times.                 |  -   | special builtin |
| [wait](https://wiki.bash-hackers.org/commands/builtin/wait)  |    Wait for background jobs and asynchronous lists.    |  -   | builtin         |