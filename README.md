sched
=====

Run a command, or schedule it if already running, with a minimum interval between runs.

Usage:

```
sched <jobname> <command> <minimum interval>
```

### Explanation

The current use case is to update the database for nsd after adding/changing DNS records. So I could have several changes come in within a short time frame, in which case I don't want or need to do the update many times, I just need to make sure the db update runs again after the last change.

### Why not Just Use the Watch Command?


The watch command is useful for monitoring the output of a command that you need to run repeatedly every n seconds.

This is not what I need to do.

What I need to do is, first of all, never run the command until I decide to schedule it.  So I may go several hours without running it.

I also don't need to continuously repeat the command.  I need to run it now, or if its already running, run it again after it is done, one more time.

The interval is just to prevent it from constantly using all of the system resources in times where my script is running a lot.

I also do not need to monitor the output or know how I would handle that output screen/modality with my script.

### Why not Just Use the 'at' Command or 'cron'?

Please read the previous section, that actually answers these questions also.  **The first thing I did was spend quite a fair amount of time trying to achieve this with 'at'**.

In case that is not enough explanation, here is some more information.  If you carefully read and understand what I am trying to do and can thinking of a simple way to do it with the 'at' or 'cron' or another command, please let me know.

My script (actually scripts) listens on a port over the VPN, updates the nsd.conf and zone file, and then needs to call nsdc rebuild. I could just do that -- put nsdc rebuild at the bottom of my script.

The problem is that I don't want to keep rebuilding the nsd database every single time a domain is added or changed -- several updates could come in within a second or two, and that is just an unnecessary load on the system to run a new rebuild for every update that comes in when they are coming rapidly.

My goal is to have enough different authoritative nameservers for different groups of zones to spread the load so that the nsd database can be rebuilt within a few seconds. Or realistically for a long time I am only going to have enough customers and domains that rebuilding the database probably will happen very quickly. I want to update the database as soon as possible.  I would definitely like to avoid waiting a full minute or two.  cron doesn't go below a minute, but at it might turn out at some point to be impractical to do that update so quickly and at that point running every 60 seconds or so might be what I have to settle for. 

However, doing it that way with cron adds one or two problems that I didn't have with the approach that is doing the updates directly in response to the change event.

Maybe the word 'schedule' and the mention of 'interval' is confusing this. **This is something I want to do immediately, I just don't want to be doing it twice in parallel if the process is already ongoing, and in that case I need to make sure it does do it again just once after the first nsdc rebuild finishes. And the interval thing is just in case it gets really busy, I want it to not be constantly running, so it can wait a second or two between runs if there are a lot of updates.**

