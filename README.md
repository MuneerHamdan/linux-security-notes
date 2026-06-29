# Muneer Hamdan overthewiresolutions
My solutions and progress logs on overthewire.org security cybersecurity concepts
_these are literally my thoughts as im going through the problems_


## bandit


### level 0
- you can ssh into another online computer using the `ssh` command
- `man ssh` to check the manpage on ssh
- on arch linux, i had to first check the status of ssh:
    * `sudo systemctl status sshd`
* it was disabled so i ran:
    + `sudo systemctl start sshd`
+ then i checked ssh help output:
    + `ssh`
+ now you can connect to the server:
    + `ssh bandit.labs.overthewire.org -p 2220`
+ i had to accept the fingerprint on my system since it was unrecognized
+ then i was brought to a prompt asking for the password
+ i kept trying to put bandit0 as the password but already noticed by reading beforehand earlier in the prompt it said something that my username was something thats not what it was supposed to be at least its not on the level 0 description
+ so i `CTRL-C`'ed out of there
+ tried again but looked in the manpage for user by doing:
    + `/user`
+ line 19 on the manpage version i was using at the time said that you connect in the form:
    + `ssh [user@]hostname or ssh://[user@]hostname[:port]`
+ got it, lets try again
+ `ssh bandit0@bandit.labs.overthewire.org -p 2220`
+ got in successfully
+ __level done__


### level 0 -> level 1
- logged in again to the host:
    * `ssh bandit0@bandit.labs.overthewire.org -p 2220`
* goal is to find the password in a file called `readme` supposedly in the home directory of the host
* when connected, list all files in current directory:
    + `ls`
+ found `readme`
+ `cat readme`
+ found the password:
    + `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`


### level 1 -> level 2
- list all files including dotfiles:
    * `ls -a`
- output didn't have a file called `-`
- confused, so made sure I didn't leave the home directory:
    * `cd ~`
* `ls -al` to see all files with verbose descriptions
* still not there
* let's try the `find` command to see if it can find it in a way that ls can't
    + prob won't change anything
    + `find . iname "-"`
+ nothing, how about:
    + `find .` to just list all files in home directory
+ same stuff as `ls -al`
+ then i realized that i was still in level 0 -> level 1's user on the host
+ so i have to ssh again and change the username to `bandit2`:
    + `exit`
    + `ssh bandit1@bandit.labs.overthewire.org -p 2220`
+ asking for a password, so I put the previously found password
+ i'm in
+ reading the intro prompt here's a couple of notes for later:
    + write-access to homedirecotries is disabled
    + so create a working directory with a hard-to-guess name in /tmp/
    + can use `mktemp -d` in order to generate a random and hard to guess directory in /tmp/
    + read-access to both /tmp/ is disabled and to /proc restricted so that users cannot snoop on eachother
    + files and directories with easily guessable or short names with be periodically deleted
+ lets see if `-` is here:
    + `ls`
+ found it
+ did `cat -` and did `<Tab>` and it autocompleted to `cat --`
+ waited for a bit but nothing was happening with just a new blank line so i did `<CTRL>-C`
+ `cat -` still was on a new blank line
+ so i think when you do `<command> -` in bash it treats it similarly to if you do something like `<command> \`
    + bc when do `<command> \` in bash it waits for user input in a similar way to whats happening here
+ `<CTRL>-C`
+ `file -` and `file *` both didn't work
+ i tried other commands like  `cat -`, `cat *`, etc. to try  to read the `-` file but they all just made the same blank line
+ then something interesting happened
+ i thought to try vim to view the contents:
    + `vim -`
+ this produced something unexpected:
    + `Vim: Reading from stdin...`
+ i would press `<CTRL>-C` three times but it showed in the line, but wouldnt stop the process
+ pressing `<ENTER>` then put me in a new Vim buffer
+ i quit vim and then looked in the linked resources
+ found doing `--` would work
+ did `cat --` but it still didn't output
+ read more, it said `./-` would work
+ did `cat ./-`
+ worked, got password
+ __level done__


### level 2 -> level 3
- `ssh bandit2@bandit.labs.overthewire.org -p 2220`
- password:
    * password found in previous level
* goal is to find the password in a file called `--spaces in this filename--`
* `ls`
* found the file
* the solution from the last level works here:
    + `cat ./--<TAB>`
    + autocompletes to `cat ./--spaces\ in\ this\ filename--`
+ found password
+ let's try another way to learn more even though we've beat the level
+ can we do it with the `cat --<TAB>` like google said?:
    + that brings up i think usage autocomplete `--<parameter>`s
    + that's not what we want
+ i guess its the preferred and cleanest way just to do `cat./<file>`
+ __level done__

### level 3 -> level 4
- so easy
- `ssh bandit3@bandit.labs.overthewire.org -p 2220`
- enter password from previous level
- `ls`
- found the `inhere` directory
- `ls inhere`
- blank output
- okay lets see all the hidden files:
    * `ls -al inhere`
* found `...Hiding-From-You`
* `cd inhere`
* `cat .<TAB>` -> `cat ...Hiding-From-You`
* found password
* __level done__
