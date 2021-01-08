# Environment variable
To pass specific environment variable to specific process only without define it in parent session
``` bash
env TZ=Asia/Shanghai date +"%Y-%m-%d %H:%M:%S %z"
scp -P 1111 locfile user@127.0.0.1:/remotepath/
```
### Bash Variables

| env                                                          | Show enviro­nment variables      |
| ------------------------------------------------------------ | -------------------------------- |
| [echo *$NAME*](http://unixhelp.ed.ac.uk/CGI/man-cgi?echo)    | Output value of *$NAME* variable |
| [export *NAME*=*value*](http://unixhelp.ed.ac.uk/CGI/man-cgi?export) | Set *$NAME* to *value*           |
| $PATH                                                        | Executable search path           |
| $HOME                                                        | Home directory                   |
| $SHELL                                                       | Current shell                    |

# Shutcuts & cmds

```bash
sudo !! # re-run previous command with 'sudo' prepended
ctrl-k, ctrl-u, ctrl-w, ctrl-y # cutting and pasting text in the command line
#use 'less +F' to view logfiles, instead of 'tail' (ctrl-c, shift-f, q to quit)
ctrl-x-e # continue editing your current shell line in a text editor (uses $EDITOR)
alt-. # paste previous command's argument (useful for running multiple commands on the same resource)
reset # resets/unborks your terminal
```



| Shortcut                   | Action                                                       |
| :------------------------- | :----------------------------------------------------------- |
| **Navigation**             |                                                              |
| `Ctrl` + `a`               | Go to the beginning of the line.                             |
| `Ctrl` + `e`               | Go to the end of the line.                                   |
| `Alt` + `f`                | Move the cursor forward one word.                            |
| `Alt` + `b`                | Move the cursor back one word.                               |
| `Ctrl` + `f`               | Move the cursor forward one character.                       |
| `Ctrl` + `b`               | Move the cursor back one character.                          |
| `Ctrl` + `x`, `x`          | Toggle between the current cursor position and the beginning of the line. |
| **Editing**                |                                                              |
| `Ctrl` + `_`               | Undo! (And, yes, that’s an underscore, so you’ll probably need to use `Shift` as well.) |
| `Ctrl` + `x`, `Ctrl` + `e` | Edit the current command in your $EDITOR.                    |
| `Alt` + `d`                | Delete the word after the cursor.                            |
| `Alt` + `Delete`           | Delete the word before the cursor.                           |
| `Ctrl` + `d`               | Delete the character beneath the cursor.                     |
| `Ctrl` + `h`               | Delete the character before the cursor (like backspace).     |
| `Ctrl` + `k`               | Cut the line after the cursor to the clipboard.              |
| `Ctrl` + `u`               | Cut the line before the cursor to the clipboard.             |
| `Ctrl` + `d`               | Cut the word after the cursor to the clipboard.              |
| `Ctrl` + `w`               | Cut the word before the cursor to the clipboard.             |
| `Ctrl` + `y`               | Paste the last item to be cut.                               |
| **Processes**              |                                                              |
| `Ctrl` + `l`               | Clear the entire screen (like the `clear` command).          |
| `Ctrl` + `z`               | Place the currently running process into a suspended background process (and then use `fg` to restore it). |
| `Ctrl` + `c`               | Kill the currently running process by sending the `SIGINT` signal. |
| `Ctrl` + `d`               | Exit the current shell.                                      |
| `Enter`, `~`, `.`          | Exit a stalled SSH session.                                  |
| **History**                |                                                              |
| `Ctrl` + `r`               | Bring up the history search.                                 |
| `Ctrl` + `g`               | Exit the history search.                                     |
| `Ctrl` + `p`               | See the previous command in the history.                     |
| `Ctrl` + `n`               | See the next command in the history.                         |

All of these keyboard shortcuts are enabled by Emacs mode in Bash. If you’d like to use Vi shortcuts instead, you can enable that mode and use [different shortcuts](https://opensource.com/article/17/3/fun-vi-mode-your-shell) instead. In addition, most of these are compatible with [Zsh](https://jbt.github.io/markdown-editor/) as well, although you might find that a few don’t work or require slightly altered shortcuts.

As promised, let’s take a slightly longer dive into some of the more complex shortcuts, particularly the ones that could benefit from some visual reinforcement as to their function.

## Command Editing Shortcuts

| `CTRL+A``CTRL+E` | Go to the start/end of the command line                      |
| ---------------- | ------------------------------------------------------------ |
| `CTRL+U``CTRL+K` | Delete from cursor to the start/end of the command line      |
| `CTRL+W``ALT+D`  | Delete from cursor to start/end of word (whole word if at the boundary) |
| `CTRL+Y`         | Paste word or text that was cut using one of the deletion shortcuts (such as the one above) after the cursor |
| `CTRL+XX`        | Move between start of command line and current cursor position (and back again) |
| `ALT+B``ALT+F`   | Move backward/forward one word (or go to start of word the cursor is currently on) |
| `ALT+C`          | Capitalize to end of word starting at cursor (whole word if cursor is at the beginning of word) |
| `ALT+U`          | Make uppercase from cursor to end of word                    |
| `ALT+L`          | Make lowercase from cursor to end of word                    |
| `ALT+T`          | Swap current word with previous                              |
| `CTRL+F``CTRL+B` | Move forward/backward one character                          |
| `CTRL+D``CTRL+H` | Delete character after/before under cursor                   |
| `CTRL+T`         | Swap character under cursor with the previous one            |

## Command Recall Shortcuts

| `CTRL+R` | Search the history backwards                                 |
| -------- | ------------------------------------------------------------ |
| `CTRL+G` | Escape from history searching mode                           |
| `CTRL+P` | Previous command in history (i.e., walk back through the command history) |
| `CTRL+N` | Next command in history (i.e., walk forward through the command history) |
| `ALT+.`  | Use the last word of the previous command                    |

## Command Control Shortcuts

| `CTRL+L` | Clear the screen                                             |
| -------- | ------------------------------------------------------------ |
| `CTRL+S` | Stops the output to the screen (for long running verbose command) |
| `CTRL+Q` | Allow output to the screen (if previously stopped using command above) |
| `CTRL+C` | Terminate the command                                        |
| `CTRL+Z` | Suspend/stop the command                                     |

## Bash Bang Shortcuts

| `!!`      | Run last command                                             |
| --------- | ------------------------------------------------------------ |
| `!blah`   | Run the most recent command that starts with `blah`          |
| `!blah:p` | Print out the command that `!blah` would run (also adds it as the latest command in the command history |
| `!$`      | The last word of the previous command (same as `ALT+.`)      |
| `!$:p`    | Print out the word that `!$` would substitute                |
| `!*`      | The previous command except for the first word (e.g., if you type `find some_file.txt /`, then `!*` would give you `some_file.txt /`) |
| `!*:p`    | Print out what `!*` would substitute                         |

### Bash Editing Shortcuts

| Ctrl + k  | delete from cursor to the end of the command line            |
| --------- | ------------------------------------------------------------ |
| Ctrl + y  | paste word or text that was cut using one of the deletion shortcuts |
| Ctrl + xx | move between start of command line and current cursor position |
| Alt + b   | move backward one word                                       |
| Alt + f   | move forward one word                                        |
| Alt + d   | delete to end of word starting at cursor                     |
| Alt + c   | capitalize to end of word starting at cursor                 |
| Alt + u   | make uppercase from cursor to end of word                    |

### Bash Bang (!) Commands

| !!      | run last command                                             |
| ------- | ------------------------------------------------------------ |
| !blah   | run the most recent command that starts with ‘blah’ (e.g. !ls) |
| !blah:p | print out the command that !blah would run                   |
| !*      | the previous command except for the last word                |
| !n      | Execute nth command in history                               |
| !$      | Last argument of last command                                |
| !^      | First argument of last command                               |
| ^abc    | Replace first occurance of abc with xyz in last command and execute it |

### Command Recall Shortcuts

| Ctrl + g | escape from history searching mode        |
| -------- | ----------------------------------------- |
| Alt + .  | use the last word of the previous command |

### Bash Editing Shortcuts 2

| Alt-Ctrl-] x | moves the cursor backwards to the previous occurance of x |
| ------------ | --------------------------------------------------------- |
| Alt-] x      | moves the cursor forward to the next occurance of x       |
| Ctrl+t       | swap character under cursor with the previous one         |
| Alt + t      | swap current word with previous                           |
| Alt + l      | make lowercase from cursor to end of word                 |

### Bash Shortcuts

| CTRL-c       | Stop current command                             |
| ------------ | ------------------------------------------------ |
| CTRL-z       | Sleep program                                    |
| CTRL-a       | Go to start of line                              |
| CTRL-e       | Go to end of line                                |
| CTRL-u       | Cut from start of line                           |
| CTRL-k       | Cut to end of line                               |
| CTRL-r       | Search history                                   |
| !!           | Repeat last command                              |
| !*abc*       | Run last command starting with *abc*             |
| !*abc*:p     | Print last command starting with *abc*           |
| !$           | Last argument of previous command                |
| ALT-.        | Last argument of previous command                |
| !*           | All arguments of previous command                |
| ^*abc*^*123* | Run previous command, replacing *abc* with *123* |

### IO Redire­ction

| *cmd* < *file*Input of *cmd* from *file*                  |
| --------------------------------------------------------- |
| *cmd1* <(*cmd2*)Output of *cmd2* as file input to *cmd1*  |
| *cmd* > *file*Standard output (stdout) of *cmd* to *file* |
| *cmd* > /dev/nullDiscard stdout of *cmd*                  |
| *cmd* >> *file*Append stdout to *file*                    |
| *cmd* 2> *file*Error output (stderr) of *cmd* to *file*   |
| *cmd* 1>&2stdout to same place as stderr                  |
| *cmd* 2>&1stderr to same place as stdout                  |
| *cmd* &> *file*Every output of *cmd* to *file*            |

*cmd* refers to a command.

### Pipes

| *cmd1* \| *cmd2*stdout of *cmd1* to *cmd2*  |
| ------------------------------------------- |
| *cmd1* \|& *cmd2*stderr of *cmd1* to *cmd2* |

### Command Lists

| *cmd1* ; *cmd2*Run *cmd1* then *cmd2*                    |
| -------------------------------------------------------- |
| *cmd1* && *cmd2*Run *cmd2* if *cmd1* is successful       |
| *cmd1* \|\| *cmd2*Run *cmd2* if *cmd1* is not successful |
| *cmd* &Run *cmd* in a subshell                           |

### File Permission Numbers

| First digit is owner permis­sion, second is group and third is everyone. |             |
| ------------------------------------------------------------ | ----------- |
| Calculate permission digits by adding numbers below.         |             |
| 4                                                            | read (r)    |
| 2                                                            | write (w)   |
| 1                                                            | execute (x) |

