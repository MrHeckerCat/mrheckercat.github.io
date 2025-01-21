---
layout: post
title: "Write Up: Introduction to Digital Forensics - Skill Assessment - HTB Academy"
date: 2025-01-20
categories: [Digital Forensics, HTB, Cybersecurity]
---

Hi again!

It's time for write-up number three, where we delve into the **Digital Forensics** module. 

Let's get started!

## Preparations

Despite initial instructions to create the artifacts using Velociraptor, I failed to create the memory dump for Volatility (other users had similar problems), so I had to use existing collections to solve all the tasks except one. This made them far less straightforward than in other modules.

It felt a bit weird since I expected to use the same tools and tactics we used for previous tasks.

---

## Task 1

To solve this one, you need to browse through logs in **Collection-J0seph-personal_localdomain**. You will see both archives on the desktop of the VM, so first, we need to extract the files.

Next, navigate to:

```
C:\Users\Administrator\Desktop\Collection-J0seph-personal_localdomain-2023-09-06T21_07_52_02_00\results
```

Open the **Windows.Packs.Persistence Startup Items.json** log file. When looking at the list of startup apps, we can immediately spot the suspicious one, reminding us of a reverse shell.

We are also going to use this file to answer question 3.

---

## Task 2

For this task, open the **Netstat logs file**:

```
Windows.Network.Netstat.json
```

It is located in the same directory as the previous task.

Look for `Raddr.IP`, as it is the target IP the machine is trying to connect to (where `192.168.152.185` is the source IP).

We spot two connections via port 80 (HTTP) that can be suspicious.

A quick check using VirusTotal revealed:
- One flagged connection
- One clean connection

Additionally, if you check other log files and some malfind results, you will see that `rundll32.exe` is flagged as suspicious.

---

## Task 3

You might have noticed a few registry keys in the file from Task 1. Since the log file name gives us a clue, we can assume these might be the answers for Task 3.

Specifically:

```
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
```

This registry key is very often used for persistence to ensure the malware runs after a computer is restarted.

It is a common question and can even be Googled without checking the logs, but always double-check when possible.

---

## Task 4

I struggled the most with this task, but once I recalled how to search for **Mimikatz** using Security logs, I managed to find the answer quite quickly.

First, you need to create an artifact using Velociraptor (this is the only task that requires this data, at least in my case).

After getting the archive and extracting it, open the **Security.evtx** log file.

Filter for **Event ID 4688** (Process creation) and search for `mimikatz` (this is very straightforward).

You will see the file path in the description.

> Be sure to include `mimik` without a slash; otherwise, the answer will not be counted as correct.

---

## Task 5

This is one of the easiest and most amusing tasks I have ever seen.

Forget about forensics; it is very plain and simple.

Navigate to:

```
C:\Users\j0seph\Desktop\Reports\reports\Washington Leak
```

Open the biggest DOCX file in the folder. The file name is your answer (yes, it is that simple). Style points for the humor though!

---

My walkthrough is not ideal, and I'm still learning, but I hope it helps people who get stuck.

Good hunting!
