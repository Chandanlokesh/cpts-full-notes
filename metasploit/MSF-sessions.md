- can run multiple module at same time that are called as sessions
- This can be done either by pressing the `[CTRL] + [Z]` key combination or by typing the `background` command in the case of Meterpreter stages.

listing the sessions
```shell-session
msf6 exploit(windows/smb/psexec_psh) > sessions
```

You can use the `sessions -i [no.]` command to open up a specific session.

### jobs
- we are running an active exploit under a specific port and need this port for a different module, we cannot simply terminate the session using `[CTRL] + [C]`
- So instead, we would need to use the `jobs` command to look at the currently active tasks running in the background and terminate the old ones to free up the port.

```shell-session
msf6 exploit(multi/handler) > jobs -h
```

When we run an exploit, we can run it as a job by typing `exploit -j`.
Instead of just `exploit` or `run`, will "run it in the context of a job."

#### Listing Running Jobs
```shell-session
msf6 exploit(multi/handler) > jobs -l
```

### kill a specific job 
`kill [index no.]`
Use the `jobs -K` command to kill all running jobs.
