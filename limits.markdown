---

layout: home
title: Limits

---

Below are the limits that are currently applied to custom code being executed through the bot. They are in place in order to prevent system misuse.

Each instance of your code gets its own set of counters to monitor its individual usage. Right now there is no premium tier, so everyone has the same limits. In the near future the ability to view an approximation of how close your instances are to reaching each limit will be added.

* Max elements in Array, Map, SMap: 1000
* Max characters in a string: 2000
* Max single write, single read database operations per instance: 10
* Max total database entries: 10 * Guild Members
* Max database size per entry: 1 KiB
* Max `sendDM` calls per instance: 1
* Max channel/thread manipulations per instance: 2
* Max state locks per instance: 100
* Max slash commands per guild: 20
* Max triggers per event type: 3

# Notes

**Max triggers per event type**

This refers to triggers such as user joined, user left, etc. Each one is its own category, and you can have 3 active triggers for each category.