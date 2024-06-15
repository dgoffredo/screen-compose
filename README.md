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
TODO:
- home server
- hack-as-you go, but restores on startup
- one file
- link to "more" section:
  - why not systemd?
  - why not docker compose?

What
----
TODO

How
---
TODO

More
----
### Why not systemd?
TODO

### Why not `docker compose`?
TODO

[1]: https://docs.docker.com/compose/
[2]: https://en.wikipedia.org/wiki/Docker_(software)
[3]: https://www.gnu.org/software/screen/
[4]: ./example
[5]: https://linux.die.net/man/5/crontab
