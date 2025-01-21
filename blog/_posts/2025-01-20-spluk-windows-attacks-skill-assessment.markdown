---
layout: post
title: "Write Up: Detecting Windows Attacks with Splunk - Skill Assessment - HTB Academy"
date: 2025-01-20
categories: [Splunk, HTB, Cybersecurity]
---

Hi again!

Welcome to write-up number 4 (at some point I'll stop counting them). This time, we are using Splunk to detect different Windows attacks. 

If you want more practice with Splunk, I highly recommend TryHackMe Splunk modules and Splunk's own **Boss of the SOC**.

Now let's start.

## Task 1

In this task, we need to modify the search from the **Detecting Beaconing Malware** module.

This is a very easy task since we just need to replace the index and remove the last two lines:

```splunk
index="empire" sourcetype="bro:http:json" 
| sort 0 _time
| streamstats current=f last(_time) as prevtime by src, dest, dest_port
| eval timedelta = _time - prevtime
| eventstats avg(timedelta) as avg, count as total by src, dest, dest_port
| eval upper=avg*1.1
| eval lower=avg*0.9
| where timedelta > lower AND timedelta < upper
| stats count, values(avg) as TimeInterval by src, dest, dest_port, total
```

Now we use the `search` command and see the beacon.

## Task 2

This task was a bit disappointing since there are only 5 events in the index and only one IP (which is also the answer).

I really encourage you to read through Splunk's detecting guide for **PrintNightmare**, which is mostly focused on Windows logs.

## Task 3

Similar to the above, the index contains only one IP address, which makes the task too easy. 

So the task really comes down to reading the guide mentioned in the task:

[Active Directory (AD) Attacks & Enumeration at the Network Layer](https://www.lares.com/active-directory-ad-attacks-enumeration-at-the-network-layer/)

---

This write-up was really short since I think this was one of the easiest modules, especially if you have previous experience with Splunk.

If you are just starting, I recommend going through TryHackMe rooms first to get acquainted with Splunk, as this HTB module is more advanced and doesn't explain the basics well.

There is also an intro module for Splunk in the HTB SOC Analyst Path, which I'm going to cover in the next write-ups.

Good hunting!
