## Lab 01

- Name: Charles Ojanama
- Email: ojanama.2@wright.edu

## Part 1 - GitHub Profile

1. [your_github_username_here's GitHub Profile](FIXTHISURL-https://github.com/tacochip)

## Part 2 - Research

| Windows | Linux / Mac | Action |
| ---     | ---         | ---    |
| help    | man         |   Manual page for command     |
| Get-Location | pwd    |    Prints current working directory   |
| Get-ChildItem | ls    |    Lists all files and directories in current location    |
| mkdir   | mkdir       |    Creates new directory    |
| Set-Location | cd     |    Changes the current directory    |
| New-Item | touch      |   Creates empty file     |
| Move-Item | mv        |    Moves file(s)    |
| Copy-Item | cp        |    Copies file(s) or directories    |
| Remove-Item | rm      |    Deletes file(s)    |
| notepad.exe | vim     |    Opens Vim text editor in terminal    |

## Part 3 - Command Line Navigation

My OS is:
- [] Windows
- [] Linux
- [x] Mac

My Command Line Shell is: Terminal

### Navigating My OS on the Command Line

1. Full / absolute path to your user's home directory: echo $HOME
2. Create a directory named `DirA`: mkdir "$HOME/DirA"
3. Create a directory named `Dir B`: mkdir "$HOME/Dir B"
4. Go into `DirA`: cd "$HOME/DirA"
5. Go into `Dir B` from `DirA`: cd "$HOME/Dir B"
6. Return to your user's home directory:cd ~
7. Create a file named `test.txt`: touch test.txt
8. Move the file named `test.txt` into `DirA`: mv test.txt "$HOME/DirA"
9. Contents of `test.txt`: echo "Yoshi Joshi" > "$HOME/DirA/test.txt"
```
Yoshi Joshi
```
10. Make a copy of `test.txt` named `copy.txt` in `DirA`: cp "$HOME/DirA/test.txt" "$HOME/DirA/copy.txt"
11. View the contents of `DirA`: ls "$HOME/DirA"
12. Make a copy of `test.txt` in `Dir B` named `fodder.txt`: cp "$HOME/DirA/test.txt" "$HOME/Dir B/fodder.txt"
13. Delete / remove both `fodder.txt` AND `Dir B`: rm "$HOME/Dir B/fodder.txt" rmdir "$HOME/Dir B"

## Citations

To add citations, provide the site and a summary of what it assisted you with.  If generative AI was used, include which generative AI system was used and what prompt(s) you fed it.

Aleksić, Marko. “Mac Terminal Commands {Cheat Sheet With Examples}.” phoenixNAP Knowledge Base, 18 May 2023, phoenixnap.com/kb/mac-terminal-commands.



