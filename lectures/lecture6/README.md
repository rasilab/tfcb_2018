# Working at the command line

Familiarity with the command line is an asset when working with computational biology data and workflows. Quite a bit of exploratory analysis of new datasets can be conducted directly from the command line. Command line exploration can also be useful for identifying the file/folder structure of data repositories and sorting out which are the key datafiles for processing in `R` or `python`. Moreover, many comp bio workflows are designed to be run from the command line.

## Key concepts: the prompt, the working directory, commands and arguments.

The **command prompt** is where you type your commands (i.e., instructions for the computer); output (if any) will appear below the command after you press enter. In the example below the prompt is `wallaby lecture6$` and the command I've typed is `pwd`, which shows what directory I'm in (the current **working directory** for command execution). This prompt is configured to show the name of the computer I'm on (`wallaby`) and the basename of the directory I'm currently in (`lecture6`). The output/result of commands executed at the command line will generally depend on the working directory in which the user is currently 'located', as explained in more detail below and as reported here by the `pwd` command. 

```bash
wallaby lecture6$ pwd
/home/pbradley/Documents/classes/tfcb_2018/lectures/lecture6
```

Here's an example where I've typed a **command** (`ls`, which lists the contents/properties of a directory or file) along with some command **arguments**: optional flags that modify the behavior of the command (`-l --sort=time`), and the directory I want to list (`./`, which is shorthand for the current directory). Note that many commands have long and short versions of flags, with the former generally being prefixed by a single dash and the latter having two dashes. Here `-l` is a short version and `--sort=time` a long; we could have used `ls -l -t` instead or even combined the shorthand flags: `ls -lt`. Many commands will print usage information when run with the `-h` or `--help` flags (see also the `man` command below).


```bash
wallaby lecture6$ ls -l --sort=time ./
total 20
-rw-r----- 1 pbradley g_pbradley 1561 Oct 15 14:35 README.md
-rw-r----- 1 pbradley g_pbradley 5609 Oct 15 14:32 tmp.html
drwxr-x--- 2 pbradley g_pbradley 4096 Oct 15 10:47 figures
-rw-r----- 1 pbradley g_pbradley 1150 Oct 15 09:11 README.md~
```

## Exploring the file system

A lot of the work we do at the command line involves processing files: reading, writing, moving, deleting, etc. Files are stored on the *file system* in a hierarchy of nested folders, through which we can navigate using commands entered at the command prompt. There are a few "special" names for describing directories and files:

* `~/` - User's home directory
* `./` - The current working directory.
* `../` - One directory up (ie, higher; opposite direction as sub-folders) in the filesystem from the current directory.
* `../../` - Two directories up. You get the idea.
* `*` -  A wildcard character that can stand in for any string of "normal" characters (e.g., not slashes). So `mv *.txt textfiles/` will move all files that end in `.txt` into the folder called `textfiles/`.
* `?` -  A wildcard character that can stand in for a *single* "normal" character.
* `/` - A path that **starts** with a `/` is an "absolute" path, defined independent of the current working directory. A path that starts with `./` or just some text is defined relative to the current directory. So `figures/cat.png` refers to a file called `cat.png` that lives in a folder `figures/` contained in the current working directory (same thing as `./figures/cat.png`). Whereas `/home/pbradley/Documents/cat.png` is a file that lives in the `Documents/` folder of my home directory (same thing as `~/Documents/cat.png`). Remember Trevor's advice: avoid the use of absolute paths in your source code and files wherever possible.
* `.*` - A filename that begins with a `.` is a hidden file which by default will not be displayed by `ls`. To show hidden files add the `-a` option: for example, `ls -a ~/` to see *all* the files in your home directory.
* `TAB` - Tab completion is a life-saver when working with long filenames. Pressing the tab key when you are in the middle of typing some pathname will complete it as far as possible given the potential matching file/directory names. Pressing `TAB` twice will display a list of possible matching paths.

### Filesystem commands

| Command | Explanation |
| --- | --- |
| `pwd` | print working directory: show the user's current directory |
| `cd` | change directory |
| `ls` | list directory or file |
| `mkdir` | make a new directory |
| `mv` | move a file or files from one place to another |
| `rm` | delete a file or directory (may be impossible to undo!) |
| `rmdir` | remove a directory (must be empty) |
| `cat` | Output the contents of a file (from "concatenate") |
| `head` | Output the head (top few) lines of a file |
| `tail` | Output the tail (last few) lines of a file |
| `more` | Look through the contents of a file one screen at a time |
| `less` | Like `more` but with line-by-line scrolling and does not leave junk on the screen after exit |
| `find` | `ls` on steroids. By default will list all files below the working directory, but `find` takes lots of optional flags and arguments that can fine-tune the search and customize the output |
| `man` | "manual"; report information on the command, as in `man ls` or `man find`|

### Now download some data and test out a few of these commands

Here's some [test data to play with](https://www.dropbox.com/s/aqb1af2p9o50qw8/lecture6_data.tgz?dl=1). Download using the link or (even better) from the command line:

``` shell
rhino1 lecture6$ wget https://www.dropbox.com/s/aqb1af2p9o50qw8/lecture6_data.tgz
```

Now you should have a file ending in the `.tgz` extension, which is a compressed "tar" archive. Once you've got it in a location you're happy with, uncompress it with the following command:

``` shell
rhino1 lecture6$ tar -xzvf lecture6_data.tgz 
...
```

Spend a few minutes fiddling with things. For example, tidy things up by making some subdirectories (aka folders) and moving the `.pdb` and `.tsv` files into their own directories.



## Getting out of trouble: ctrl-C (aka ^C), ctrl-d

Sometimes you need to exit a command that hangs or is taking too long or will never return at all because of a typo. `ctrl-c` aka `^c` aka `control-c` (i.e., hold the control key and press 'c'; think 'c' as in cancel) will generally do the trick. 

``` shell
wallaby lecture6$ tail
^C
wallaby lecture6$ grep "test"
^C
```

ctrl-d (aka EOF) is also used for making a controlled exit from commands taking text input (e.g. `cat`).

``` shell
rhino1 lecture6_data$ cat > README.txt
This text here will go into the
file called README.txt
^D
rhino1 lecture6_data$ 
# I entered ctrl-D after the second line to exit cat.
# See next section for meaning of the > symbol.
```

## Redirecting and 'piping' the output of commands

By default, most commands will send their output to the screen below the prompt (aka *standard output* or 'stdout'), but you can change this to send the output to a file or even another command. The command receiving the input is said to be reading from *standard input* (aka 'stdin').

* `>` : Redirects the output to a file; will overwrite the file if it exists. 
* `>>` : Redirects the output to a file; will append to the file if it exists. 
* `|` : 'Pipe' symbol, sends output to another command.

``` shell
wallaby lecture6$ ls figures > figure_list.txt
wallaby lectures$ ls lecture6/figures/*png >> all_png_files.txt
```

Count all the figures in the `figures/` folders, using the `ls` command followed by the `wc` (word count) command:

``` shell
wallaby lectures$ ls lecture*/figures/* | wc
      3       3     106
```

## Running commands in the background: &, ctrl-z

You don't have to wait patiently for a command to finish: if you add an `&` at the end of the line before hitting enter, the command will run and you will get the prompt back. If the command is going to generate any output it's helpful to redirect or pipe that so that it doesn't spill out onto the screen while you are typing your next command.


```shell
wallaby lecture6$ python run_complicated_analysis.py > results.txt &
```

If you type a command and realize that you *should* have typed an `&` at the end, you can type `ctrl-z` to get the prompt. That will suspend (pause) the command. To start it running again in the background, type `bg` and press enter. The `jobs` command will show a list of the background jobs running in that terminal session. 

The `kill` command can be used to kill a process that is running in the background. You need to find the process ID (PID) of the job, which you can do with the `ps` command:


```shell
rhino1 tcr$ nice /home/pbradley/bin/rosetta.exe @job128.args -unmute apps > logfile.txt &
[1] 56215
rhino1 tcr$ ps -u
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
pbradley  1576  0.0  0.0 174752  1052 ?        S    Aug22   3:00 sshd: pbradley@pts/173
pbradley  1577  0.0  0.0  26224  6092 pts/173  Ss   Aug22   0:19 -bash
...
pbradley 56215 64.0  0.1 515004 418216 pts/173 SN   07:34   0:08 /home/pbradley/bin/rosetta.exe @job128.args -unmute apps
...
pbradley 56353  0.0  0.0  16076  1220 pts/173  R+   07:34   0:00 ps ux
rhino1 tcr$ kill 56215
```

## History, re-using commands

To re-run a command that you used recently, you can use the up and down arrows to navigate through your command history. When you see the command you want, you can either press `enter` to re-run it unaltered, or edit it with the cursor and then press enter. `history` is a really useful command that will print out the list of all the commands you've entered. You can redirect that output to a file to store a record of what you did, and edit the file to produce a list of commands that can be re-run using the `source` command (which executes all the commands in the input file) 


``` shell
rhino1 tcr$ history | tail 
 1936  jobs
 1937  fg
 1938  python ~/gitrepos/tcrmodel/python/ajob79.py --logfile tmp127.fd -p 0.05 -s ddg -c 1 --notargetsort  --nocluster
 1939  python ~/gitrepos/tcrmodel/python/ajob79.py --logfile tmp127.fd -p 0.05 -s ddg -c 1 --notargetsort  --nocluster
 1940  python ~/gitrepos/tcrmodel/python/ajob79.py --logfile tmp127.fd -p 0.05 -s ddg -c 1 --peptargetsort  --nocluster
 1941  python ~/gitrepos/tcrmodel/python/ajob79.py --logfile tmp127.fd -p 0.05 -s ddg -c 1 --peptargetsort  --nocluster
 1942  bg
 1943  history 
 1944  history 
 1945  history | tail 
# re-run command number 1940 from the history
rhino1 tcr$ !1940
python ~/gitrepos/tcrmodel/python/ajob79.py --logfile tmp127.fd -p 0.05 -s ddg -c 1 --peptargetsort  --nocluster
```

``` shell
rhino1 tcr$ history | tail -100 > recent_history.txt
rhino1 tcr$ cp recent_history.txt figure_commands.txt
# edit figure_commands.txt to select the commands and remove the line numbers (could also do this with awk or a script)
rhino1 tcr$ vi figure_commands.txt
# sometime later... source the file to re-run all the commands
rhino1 tcr$ source figure_commands.txt
```

**TIP**: `ctrl-r` is an awesome keyboard shortcut that lets you search backward through history for a command containing a string (the text you type after typing ctrl-r).


## A few more useful commands for analysis from the command line

| Command | Explanation |
| --- | --- |
| `grep` | Search for string or regular expression within file(s) or `stdin` (output piped from other commands) |
| `sort` | Sort file or stdin (alphabetically or numerically, choice of fields for sorting) |
| `uniq` | Remove successive duplicate lines from files or stdin, with `-c` option useful for counting things |
| `wc` | Count lines/words/characters |
| `diff` | compare two files, showing differences |
| `cut` | Display only selected fields/characters from files/stdin |
| `awk` | Awesome general-purpose scripting language for quick and dirty command line manipulations |

### Now spend a few more minutes playing with the data you downloaded earlier

For example:

* Find the largest `.pdb` file, by size in bytes and also by number of lines and words. Hint: try `ls -l` and `wc`
* Find the pdb file that has the greatest number of water molecules ('HOH' is the symbol for water in these protein structure coordinate files). Hint: `grep` is the one to use here, but inspect the output first. The `-c` option counts the number of times  a pattern was found in each input file. 
* Find the pdb file with the best (smallest) crystallographic resolution. Hint: `grep` for `RESOLUTION` in those files, try sorting.
* Are there duplicates in the `amino acid` field in any of the .tsv files? Hint: `cut` is your friend here. `cut -f 1` will output just the first field, `cut -f 1,2` prints fields 1 and 2. The default *delimiter* for `cut` is tab, but you can change that with the `-d` option (not necessary in this case though).
* Are there amino acid or nucleotide sequences that occur across .tsv files? Hint: try `cut` and `uniq`...

## Commands are just files; how `PATH` works

All the commands we've talked about (`ls`, `pwd`) are actually just *executable* files that live somewhere on the file system. When you type a command like `ls`, the shell searches through an ordered list of locations (directories) looking for an executable called `ls`, and stops at the first match. This ordered list of search directories is called the **path** and is stored in an environment variable called `PATH`. You can see the current path by using the command `echo` (which just prints stuff) with the variable name preceded by `$`:

``` shell
rhino1 lecture6_data$ echo $PATH
/home/pbradley/anaconda2/bin:/app/bin:/app/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/home/pbradley/bin:/home/pbradley/local/python_packages/bin/:/home/pbradley/local/bin/

```

The command `which` shows where commands live:

``` shell
rhino1 lecture6_data$ which ls
/bin/ls
```

when installing and running new software, we have to make sure that the new executables can be found. We can do this by copying/sym-linking them into one of the directories in the current path, by adding a new directory to the path, or by calling them with an absolute pathname.

``` shell
# this doesn't work:
rhino1 lecture6_data$ seqlogo -h
seqlogo: command not found

# but this does:
rhino1 lecture6_data$ /home/pbradley/download/weblogo/weblogo/seqlogo -h
usage: seqlogo -f <input filename> [OPTIONs with values]
Creates a logo for the given input filename.
...

# we can add the directory where seqlogo lives to our path using the export command:
rhino1 lecture6_data$ export PATH=$PATH:/home/pbradley/download/weblogo/weblogo/
rhino1 lecture6_data$ echo $PATH
/home/pbradley/anaconda2/bin:/app/bin:/app/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/home/pbradley/bin:/home/pbradley/local/python_packages/bin/:/home/pbradley/local/bin/:/home/pbradley/download/weblogo/weblogo/

# and now it works:
rhino1 lecture6_data$ seqlogo -h
usage: seqlogo -f <input filename> [OPTIONs with values]
Creates a logo for the given input filename.
...
```

Including `./` in the PATH is generally agreed to be a bad idea, as this example illustrates:


``` shell
# put ./ (the current directory) as the first element in PATH [do not do this!]
rhino1 lecture6_data$ export PATH=./:$PATH

# now create a file called ls in this directory
rhino1 lecture6_data$ cat > ls
#!/bin/bash
echo "no files here"
^D

# ls still works normally since this new ls file is not yet executable
rhino1 lecture6_data$ ls
1ao7.pdb  1fyt.pdb  1mi5.pdb  1qrn.pdb  1qsf.pdb  lecture6_data      ls          rep00_top_10000.tsv  rep02_top_10000.tsv
1bd2.pdb  1j8h.pdb  1oga.pdb  1qse.pdb  1ymm.pdb  lecture6_data.tgz  README.txt  rep01_top_10000.tsv  tmp.pdb_tcr_human.053118.log

# change the permissions to make our new ls executable:
rhino1 lecture6_data$ chmod +x ls

# now try again
rhino1 lecture6_data$ ls
no files here

```

Luckily this change I made to the $PATH environment variable only applies to my current shell process. When I start another process, for example by opening a new `Terminal` window, `PATH` is back to normal. So, how do I make permanent changes to things like my path? Proceed to the next section...

## Configuring: ~/.bashrc, alias, export

There are a number of configuration files that get processed when starting a new command line session. The specifics are somewhat system dependent, but assuming that you are running the `bash` shell (a typical default), the file `~/.bashrc` is a good place to put commands that should be run before starting a command line session. For example, if you want to add a directory called `bin` in your home directory to the executable search path, you could put the following line in your `~/.bashrc`:

``` shell
export PATH=$PATH:$HOME/bin
```

Here we used the `$HOME` variable as a shorter and more general name for our home directory. In addition to the path we might want to fiddle with the prompt:

``` shell
# PS1 is the environment variable that determines the prompt: \h is short for the hostname and
# \W gives the basename of the current working directory
PS1='\h \W$ '
```

or set the default editor

``` shell
export EDITOR='emacs -nw'
```

The command `env` will list all the current environment variables and their settings.

Another really handy configuration uses the `alias` command to create new shorthands for frequently used command/argument combinations. For example



``` shell
# new will list the newest 10 files in the current directory
alias new='/bin/ls -lt --color | head'

# grist combines history and grep
alias grist='history | grep '

# sort numerically and don't need to type -k for the field
alias nsort='sort -n -k '

# never use vi, only emacs
alias vi='emacs -nw'

```

Finally, it's worth mentioning the concept of symlinks, which allow us to put a pointer to a file or directory in a new place without actually copying or moving the old version. This can be handy for getting executable files into our path:

``` shell
ln -s /home/pbradley/download/weblogo/weblogo/seqlogo ~/bin/
```

will add a *symlink* to the `seqlogo` executable into the directory `~/bin`. As with all these commands, type `man ln` or `ln --help` or google "linux ln command" to get all the gory details.

## Shell scripts

Earlier on we talked about dumping a bunch of commands from our history into a file and then re-running these commands using `source`. An even nicer way to do this kind of thing is to create a shell script, which is an executable file that can be run just like any other command.


``` shell
# create a file with commands and a header line that tells the OS what program 
# to use to execute this script:
rhino1 tfcb_2018$ cat > get_data.sh
#!/bin/bash
wget https://www.dropbox.com/s/aqb1af2p9o50qw8/lecture6_data.tgz
tar -xzvf lecture6_data.tgz
^D

# make the file executable
rhino1 tfcb_2018$ chmod +x get_data.sh 

# run the file like a normal command (./ since the current directory is not in our PATH)
rhino1 tfcb_2018$ ./get_data.sh 
--2018-10-16 13:46:10--  https://www.dropbox.com/s/aqb1af2p9o50qw8/lecture6_data.tgz
Resolving www.dropbox.com (www.dropbox.com)... 162.125.1.1, 2620:100:6016:1::a27d:101
Connecting to www.dropbox.com (www.dropbox.com)|162.125.1.1|:443... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: /s/raw/aqb1af2p9o50qw8/lecture6_data.tgz [following]
... [etc]
```

This works for python, too:

``` shell
# create the file
rhino1 tfcb_2018$ cat > silly.py
#!/home/pbradley/anaconda2/bin/python
print 'hello'
^D

# make it executable
rhino1 tfcb_2018$ chmod +x silly.py 

# run it
rhino1 tfcb_2018$ ./silly.py 
hello

# copy to a directory in my path
rhino1 tfcb_2018$ cp silly.py ~/bin

# run it without the ./
rhino1 tfcb_2018$ silly.py 
hello

# clean up
rhino1 tfcb_2018$ rm ~/bin/silly.py 

```

## Text editors

* `vi` and `vim`: Becoming unexpectedly trapped in a `vi` session can be pretty terrifying. But it's actually not that scary if you know a few basics. First of all, vi has two modes: command-mode and insert-text-mode. To get into command mode hit `Esc`, to get from command mode into insert-text-mode hit `i`. In `insert-text-mode` you can surf around and edit the file. From command mode you can exit by typing `:wq[enter]` or 'ZZ' -- I think these save the current file. But to be safe it's worth checking a `vi` tutorial of some kind. 

* `emacs -nw`: I love emacs. There's a bit of a learning curve but it's very configurable. The flag `-nw` here means don't open a window, ie operate within the terminal.

* `nano`: Seems like a pretty bare-bones editor.


## Downloading files from the web; archiving/transferring files
| Command | Explanation |
| --- | --- |
| `wget` and `curl` | Downloading files from the web |
| `scp` and `rsync` | Transferring files between computers/file systems/etc |
| `tar` and `zip` | Archiving and extracting folders of files; file extensions: .tar/.tgz/.tbz2 (tar) .zip (zip)|
| `gzip` and `bzip2` | Compressing and decompressing files; file extensions .gz (gzip) and .bz2 (bzip2) |

## Two handy commands that might be worth exploring in more detail if you do a lot of work from the command line.

### `awk`

I get by with a very basic knowledge of `awk` but still find it super-useful. It has a C-style syntax with if-then-else, printf, but also things like associative arrays which are very handy. Here are a couple of examples:


``` shell
# Example 1: Print the 3rd white-space delimited field in `file.txt`

awk '{print $3}' file.txt

# Example 2: Print the header line of file.txt with each field on its own separate line, numbered:

head -1 file.txt | awk '{for ( i=1; i<= NF; ++i ) {print i,$i}}'

# Example 3: Get the name of all the directories in the current directory:

ls -l | awk '(/^d/){print $NF}'

# Example 4: Count up how many files each owner has in this directory:

ls -l | awk '(!/^total/){owner = $3; ++count[owner]}END{for ( owner in count ) {print owner, count[owner]}}'
```

Awk statements basically consist of tests, which check to see whether a given line of input satisfies some condition (and can be empty), followed by actions: `awk '(test){action}'`. `action` is executed for all input lines that satisfy the test. In the above examples, #1 and #2 don't have a test (they act on all lines on input), while #3 looks for lines that begin with the letter d (`^d` is a regular expression that looks for d at the beginning), and #4 looks for lines that *don't* begin with the string `total` (`ls` generates a `total` line that we don't want to parse).

The variable `NF` tells how many fields are in the current line (see #2 and #3). In example #4, `count` is an associative array (like a `python` dictionary) that counts the number of files for each file owner. `END` is a test that only returns true at the end of the input, once all the lines have been processed. Similarly `BEGIN` can be used to specify an action that should be performed before parsing any input lines.

[This seems like an informative Awk tutorial](http://www.grymoire.com/Unix/Awk.html)

### `xargs`

`xargs` takes all the stdin (piped from another command, typically) and packages it into arguments for a third command. 

For example: run `wc` on all the pdb files contained anywhere in this folder or its subdirectories:

``` shell
rhino1 tfcb_2018$ find . | grep "\.pdb$" | xargs wc
    6547    74673   530307 ./lecture6_data/1ao7.pdb
    7062    82270   572022 ./lecture6_data/1bd2.pdb
    7331    85525   593811 ./lecture6_data/1fyt.pdb
    7639    88950   618759 ./lecture6_data/1j8h.pdb
    7457    87513   604017 ./lecture6_data/1mi5.pdb
    8027    92677   650187 ./lecture6_data/1oga.pdb
    7206    84247   583686 ./lecture6_data/1qrn.pdb
    7156    82826   579636 ./lecture6_data/1qse.pdb
    7163    82871   580203 ./lecture6_data/1qsf.pdb
    6974    77335   564894 ./lecture6_data/1ymm.pdb
```

For example: cancel all my pending (PD) jobs on the cluster using the scancel command:

``` shell
squeue | grep pbradley | grep PD | awk '{print $1}' | xargs scancel
```

## Permissions

Every file or directory has three permissions (read/write/execute) for three different users (owner/group/everybody else). You can set these with the `chmod` command:

``` shell
# allow everyone to read ('r') and execute ('x') script.py
chmod a+rx script.py

# don't let anyone other than owner (and their "group") look at or enter this folder:
chmod a-rx folder
```

Permissions can be super-frustrating and confusing. One gotcha is that directories need to be executable for people to look inside.

``` shell
rhino1 lecture6_data$ chmod a-x lecture6_data
rhino1 lecture6_data$ ls -ld lecture6_data
drw-r----- 2 pbradley g_pbradley 4096 Oct 16 09:45 lecture6_data
rhino1 lecture6_data$ cd lecture6_data/
-bash: cd: lecture6_data/: Permission denied
```

Here `ls -l` shows the permissions for a file or folder in the first column. After the `d` which means directory, the order is `rwxrwxrwx` for owner,group,other respectively.


## Compiling from the command line

Sometimes we need to install software from the command line. This can be scary but usually turns out to be pretty straightforward. A typical linux install involves the following steps (borrowing from this [guide](https://www.control-escape.com/linux/lx-swinstall-tar.html)):

``` shell
[root]# tar -xzvf ${name}.tar.gz
Unpacking ...
[root]# cd ${name}
[root]# more README
[root]# more INSTALL
[root]# ./configure
[root]# make
[root]# make install
```

where `${name}` is the name of the package directory. An issue may arise on systems where we don't have `root` permissions (needed to modify some of the system directories for installing some of the package files). In these situations we can often add the `--prefix` option to the `./configure` command, which will install the software in a location where we do have write permissions.

``` shell
rhino1 lecture6_data$ ./configure --prefix=/home/pbradley/local
```

### Further reading

* [Unix cheatsheet](http://cheatsheetworld.com/programming/unix-linux-cheat-sheet/)
* [Another unix cheatsheet](http://files.fosswire.com/2007/08/fwunixref.pdf)
* [Command-line tutorial](http://www.learnenough.com/command-line-tutorial)
* [Another command-line tutorial](https://www.vikingcodeschool.com/web-development-basics/a-command-line-crash-course)

