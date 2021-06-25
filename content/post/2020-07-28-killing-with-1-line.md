---
date: "2020-07-28T00:00:00Z"
subtitle: Powerful use of piping in command line
tags:
- random
title: Killing process with one command, which is still utilizing the port
---

# Killing the process
It happens regularly that when you stop the process(in my case mostly a node server or flask server) and restart in the same terminal session. It gives the error saying
`PORT is still in use`. In that case, using `ps aux` and `grep`, you find the process id, and kill it. In [this](https://stackoverflow.com/questions/10522532/stop-node-js-program-from-command-line) stackoverflow answer by Hamid Tavakoli.
This simple one line does it for you
```
kill -9 $(ps aux | grep '\snode\s' | awk '{print $2}')
```
the above serves as an example for killing an node js process, which is still utilizing the port. It depicts the powerful use of piping. the `\s` on both sides of node is for whitespaces and `$2` fetches the pid using awk.
