# ssh-util

#### ToDo
* Feature: copy and paste filesystem entries relative to the specified remote dir and the current local dir.
* Feature: include copying from remote location to remote location.

### What is it?
A simple bash script that allows you to automate your ssh connections through aliases. 
It uses an expect script command to automate the process of login, and the initial navigation to the desired directory on the server.

It also allows for a remote folder to be set. Once logged in, the script will execute a "cd <path of choice>" command to so you can have pre-set remote directories which you can access via an arbitrary alias.

There are two things: the main script "sshutil"; the config file "$HOME/.sshutil/config". The config file should be placed in the home directory under the aforementioned path. The script itself can be placed anywhere.

### Execute Command
Connect to alias:
~~~~
/path/to/sshutil Alias
~~~~
List all aliases:
~~~~
/path/to/sshutil -l
~~~~

### Requirements

* Bash -- obviously.
* SSH -- obviously.
* Expect.
* Scp.

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

### Setup
The contents of the config file are retrieved to define specified variables: ALIAS, USER, HOST, PASS, PROMPT, and REMOTEDIR. 
**Note:** the PASS variable now supports commands so passwords can be retrieved using a password manager -- see example with alias "my-website".

These can be specified with the following syntax:

~~~~
ALIAS myserver 
USER myusername 
HOST myhost
PASS apassword
PROMPT mybashcliprompt
REMOTEDIR myremotedirectory

ALIAS my-website
USER mike
HOST 192.168.0.1
PASS -- pass server/mike
PROMPT mike-laptop$
REMOTEDIR /var/www/

ALIAS my-safe
USER alan 
HOST 195.62.0.40
PASS so-insecure-to-leave/passwords-unencrypted-in-a-file
PROMPT alan@mike-server$
REMOTEDIR /home/alan/stash/
~~~~

* **ALIAS** -- the arbitrary name of the configuration.
* **USER** -- the usernamer to use for remote login.
* **HOST** -- the domain for the server.
* **PASS** -- the password used for the key passphrase or server password (OPTIONAL).
* **PROMPT** -- the regular expression for the script to identify that the user has logged in (OPTIONAL). Default value is "\$\s*"
* **REMOTEDIR** -- the remote directory to which the script should navigate.

**Note:** The above example contains identical formatting with a practical version. Each configuration should be separated by a new line, and HAS to start with ALIAS as the property/key on its first line. Otherwise the script will skip to the next set of config properties. 
**Note:** For security purposes, the PASS value can be omitted. The script will then ask for one when prompted.
