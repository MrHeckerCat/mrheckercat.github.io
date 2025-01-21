---
layout: post
title: "Write Up: YARA & Sigma for SOC Analysts - Skill Assessment - HTB Academy"
date: 2025-01-20
categories: [YARA, Sigma, SOC, HTB, Cybersecurity]
---

Hello there!

I'm back with my second write-up, even though I don't think anyone actually read the first one (at least that's what Medium stats say).

I keep exploring **YARA** and **Sigma**, and this time I will go through:

**YARA & Sigma for SOC Analysts - Skill Assessment Module**

---

## Task 1

Let's start with the first task that asks us to find the string to complete the YARA rule:

### Step 1: Using strings command to get strings for Seatbelt.exe

After starting the VM and connecting to it using RDP, we need to open **PowerShell as Admin** (since our VM is a Windows machine, we are going to use PowerShell in this module).

Then, enter the following command to get all strings from `Seatbelt.exe` and save them to a text file:

```powershell
strings C:\Samples\YARASigma\Seatbelt.exe > seatbelt.txt
```

### Step 2: Using regular expressions to get the list of strings

Since we have a hint to look for a specific string that starts with **"L"** and ends with **"r"**, we can run a command with a regular expression that will show us all the strings matching the criteria:

```powershell
Select-String -Path "C:\Samples\YARASigma\seatbelt.txt" -Pattern "\bL\w*r\b" -AllMatches | ForEach-Object { $_.Matches } | ForEach-Object { $_.Value }
```

We will get the list with one of the strings that is the answer.

### Why "LsaWrapper"?

**LSA** is short for "Local Security Authority" and is a crucial component of Windows security.

Seatbelt, being a Windows pentest/security tool, utilizes LSA to get access to user/admin credentials.

---

## Task 2

Now let's move to the second task where we need to use **Sigma** and **Chainsaw**.

This is a pretty simple task. Navigate to:

```powershell
C:\Tools\chainsaw
```

Run the following command:

```powershell
.\chainsaw_x86_64-pc-windows-msvc.exe hunt C:\Events\YARASigma\lab_events_6.evtx -s C:\Tools\chainsaw\sigma\rules\windows\powershell\powershell_script\posh_ps_susp_win32_shadowcopy.yml --mapping .\mappings\sigma-event-logs-all.yml
```

After running the command with the rule, we will see the detection details.

You will also see the **ScriptBlockId**, which is the answer for the task above.

---

Hope this helps. Good hunting!
