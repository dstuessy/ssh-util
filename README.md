# ssh-util

### ToDo
* copy and paste filesystem entries relative to the specified remote dir and the current local dir.

### Requirements

* Bash -- obviously.
* SSH -- obviously.
* Expect.

Expect script can be found as a supported package in many Linux distributions: https://packages.debian.org/sid/expect and https://www.archlinux.org/packages/extra/i686/expect/

For debian-based distributions: 
~~~~
sudo apt-get install expect
~~~~
For Arch linux: 
~~~~
sudo pacman -S expect
~~~~

The above two commands have not been tested, please revise your distribution's package manager to see which package to install. 

### What is it?
A simple bash script that allows you to automate your ssh connections through aliases. 
It uses an expect script command to automate the process of login, and the initial navigation to the desired directory on the server.

It also allows for a remote folder to be set. Once logged in, the script will execute a "cd <path of choice>" command to so you can have pre-set remote directories which you can access via an arbitrary alias.

There are two things: the main script "sshutil"; the config file "<name and path of choice>". The config file can be placed anywhere, as its path must be specified under the variable FILENAME in the "sshutil" script. 

The contents of the config file are retrieved to define specified variables: ALIAS, USER, HOST, EXPECT, USE_PASS or USE_KEY, PROMPT, and REMOTEDIR. 

These can be specified with the following syntax:

~~~~
ALIAS myserver 
USER myusername 
HOST myhost
USE_PASS true
USE_KEY true
PROMPT mybashcliprompt
REMOTEDIR myremotedirectory

ALIAS my-website
USER mike
HOST 192.168.0.1
USE_PASS true
PROMPT mike-laptop$
REMOTEDIR /var/www/

ALIAS my-safe
USER alan 
HOST 195.62.0.40
USE_KEY true
PROMPT alan@mike-server$
REMOTEDIR /home/alan/stash/
~~~~

* **ALIAS** -- the arbitrary name of the configuration.
* **USER** -- the usernamer to use for remote login.
* **HOST** -- the domain for the server.
* **USE_PASS** or **USE_KEY** -- specify if the ssh connection will require a password or keyphrase.
* **PROMPT** -- the characters for the script to identify that the user has logged in.
* **REMOTEDIR** -- the remote directory to which the script should navigate.

**Note:** The above example contains identical formatting with a practical version. Each configuration should be separated by a new line, and HAS to start with ALIAS as the property/key on its first line. Otherwise the script will skip to the next set of config properties. 

It should also be noted that USE_PASS and USE_KEY can be placed in the config properties, but the script will use the properties of the one that is furthest down, i.e. if USE_PASS is below USE_KEY, the script will asume SSH is going to request a password, not a key passphrase.
