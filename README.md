`screen-compose`
================
It's like [docker compose][1], but instead of containers in [Docker][2], it's
command shells in [GNU Screen][3].
```console
david@carbon:~/src/screen-compose$ ./screen-compose example
david@carbon:~/src/screen-compose$ screen -list
There is a screen on:
        55734..carbon   (06/15/2024 02:09:26 AM)        (Detached)
1 Socket in /run/screen/S-david.
david@carbon:~/src/screen-compose$ screen -r
```
Now you're attached to a `screen` session that's running all of the shells
defined in the [example][4].

You can even run `screen-compose` as a [@reboot][5] `cron` user job, and attach
to it later over `ssh`. That's the idea.

Why
---
I have various websites, side projects, and home automation programs running on
an old laptop that I use as a home server. Whenever that server reboots, I have
to remember what was running on it.

There are many solutions to this problem, but I want one that feels as close as
possible to me setting things up by hand every time.  I want to automate
opening up a bunch of shells and typing commands into them.

What
----
[screen-compose][6] is a Python script that reads a configuration file and then
executes `screen` in detached mode. The configuration file specifies how many
windows the screen session will have, their titles, and initial shell commands
to type into each window.

The configuration file format is described in the [example][4] configuration.

How
---
```console
david@carbon:~/src/screen-compose$ ./screen-compose --help
usage: screen-compose [-h] [-S SESSION_NAME] [--lint] config_file

Set up a GNU Screen session.

positional arguments:
  config_file           path to configuration file

options:
  -h, --help            show this help message and exit
  -S SESSION_NAME, --session-name SESSION_NAME
                        custom name for the session
  --lint                validate configuration without running GNU screen

david@carbon:~/src/screen-compose$ ./screen-compose ./example
david@carbon:~/src/screen-compose$ screen -r
```
```console
python read.py
python read.py
python read.py
python read.py
python read.py








 [0 asus probe]  1 lookatmyplants.com f  2 fowzystuff.com mover  3 shell
```
One way to have it run on boot:
```console
david@denmark:~$ crontab -l | grep '@reboot'
@reboot         cd ~/src/screen-compose && ./screen-compose --session-name services ./services >>log 2>&1
david@denmark:~$
```

More
----
### Why not systemd?
I don't want to use `service`, `systemctl`, and `journalctl` to manage my pet
projects.  I want to get in there with my hands and poke around with a shell.

### Why not docker compose?
I don't want to use `docker compose {attach,exec,logs}` to manage my pet
projects.  I want the scripts and services to run on the system outside of any
container.

### Why not GNU Shepherd?
I just want to automate setting up a screen session, ok?

### How does it work?
When GNU Screen starts, it reads a file of commands. By default it reads
`~/.screenrc`, but the file path can be overridden via the `-c` command line
option.

screen-compose creates such a file and passes it to `screen`. One file is not
enough, though, because in order to reliably execute large amounts of shell
commands in a screen window, the commands must be read from a separate file.
So, screen-compose creates a temporary directory containing multiple files,
executes `screen`, and later deletes the temporary directory.

[1]: https://docs.docker.com/compose/
[2]: https://en.wikipedia.org/wiki/Docker_(software)
[3]: https://www.gnu.org/software/screen/
[4]: ./example
[5]: https://linux.die.net/man/5/crontab
[6]: ./screen-compose
