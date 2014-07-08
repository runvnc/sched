sched
=====

Run a command, or schedule it if already running, with a minimum interval between runs.

Usage:

```
sched <jobname> <command> <minimum interval>
```

Explanation
===========

The current use case is to update the database for nsd after adding/changing DNS records. So I could have several changes come in within a short time frame, in which case I don't want or need to do the update many times, I just need to make sure the db update runs again after the last change.

Why not Just Use the Watch Command?
===================================

The watch command is useful for monitoring the output of a command that you need to run repeatedly every n seconds.

This is not what I need to do.

What I need to do is, first of all, never run the command until I decide to schedule it.  So I may go several hours without running it.

I also don't need to continuously repeat the command.  I need to run it now, or if its already running, run it again after it is done, one more time.

The interval is just to prevent it from constantly using all of the system resources in times where my script is running a lot.

I also do not need to monitor the output or know how I would handle that output screen/modality with my script.

