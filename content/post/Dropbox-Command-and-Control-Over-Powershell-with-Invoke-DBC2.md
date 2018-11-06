---
title: "Dropbox Command and Control Over Powershell With Invoke DBC2"
date: 2017-03-03T07:02:14+01:00
draft: false
---

Consider a scenario where a Penetration Tester is trying to set up command and control on an internal network blocking all outbound traffic, except traffic towards a few specific servers or 3rd party File Sharing websites. In this situation, there are still a few options a tester can use. One of those is DNS command and control and the other is Command and control over file sharing sites such as Dropbox and Google docs. In this blog post we will explore how Dropbox can be used for such operations and more.

In case you're unfamiliar with this concept, it involves using DropBox as a platform for command and control since Dropbox is a legitimate website for storing files and documents. This concept makes it incredibly hard for users to immediately suspect and report malicious activities on their systems, therefore making C2 over DropBox highly effective.

[DropBox-C2(DBC2)](https://github.com/Arno0x/DBC2) by [@Arno0x0x](https://twitter.com/arno0x0x) is the best example I've 
come across for such a working example. It supports running commands, shellMode for a more interactive feel, launching processes, 
Persistence, Data Exfiltration and running Custom Powershell modules.

The last feature I was really excited about because it gives testers many possibilities including running their 
own custom Powershell scripts in Memory without touching disk. The agent is written in C-Sharp and the server is written in Python.

After a week and a half, I managed to implement some features of the DBC2 agent in Powershell. I have been teaching myself Powershell over the last month and thought I'd take on this challenge to further my knowledge of Powershell since it’s quite common among APTs and Penetration Testers. 
This is due to its versatility, many features and the fact that it’s inbuilt in Windows systems. 
In this blog post we'll look at how the DBC2-Powershell script can be used

# Setup

You can read how to setup and use the original [DBC2](https://github.com/Arno0x/DBC2), its a fantastic resource 
and I highly recommend it. Once you've setup the server and its ready, you can start it like this:

	python dropboxc2.py


The server will then ask for your DropBox Access token and master password to encrypt all data between the agent and server controller.
A windows machine with Powershell version 5.0 is required. The DBC2 script can be loaded by downloading the script and running it as shown:

	. .\Invoke-DBC2.ps1

Once the script is loaded run the following command to start the DBC2 Powershell Agent:

	Invoke-DBC2

Invoke-DropBox is the name of the Main function used in DBC2-Powershell. After its executed it will create two files on your Dropbox account. A status file and a command file each with a unique identifier based on each machine its run on.  

# Executing Native Commands

From the server you can direct the client to perform and run various commands.
Here's a video of what that looks like:

[![DBC2-Powershell-RunCmds](https://img.youtube.com/vi/srddn2BBKIQ/0.jpg)](http://www.youtube.com/watch?v=srddn2BBKIQ)

# Launching Processes

From the server side you can have the agent launch processes. In this case we'll be launching notepad and chrome.

[![DBC2-LaunchProcesses](https://img.youtube.com/vi/xi9E8f6ybc4/0.jpg)](http://www.youtube.com/watch?v=xi9E8f6ybc4)

# File Transfers

The script can transfer files from the controller to the server and vice versa.
The short video shows how to send a file from the controller to the agent:

[![Send File](https://img.youtube.com/vi/azdi8yJmrRM/0.jpg)](http://www.youtube.com/watch?v=azdi8yJmrRM)

The short video shows how to send a file from the agent to the controller:

[![Get File](https://img.youtube.com/vi/fUx3EXkzJ2M/0.jpg)](http://www.youtube.com/watch?v=fUx3EXkzJ2M)

# Using Powershell Modules

DBC2-Powershell Agent has the ability to run custom Powershell and return the output to the server.
It still has a few kinks which I am currently working to fix but for now it can execute the scripts and return output just fine. In the video below we will use PowerDump to dump local password hashes from the local system:

[![DBC2-PSmodules](https://img.youtube.com/vi/7U7Vs7xVQGc/0.jpg)](http://www.youtube.com/watch?v=7U7Vs7xVQGc)

# Persistence

Unlike the original DBC2 agent which relied on scheduled tasks with a malicious bat file for persistence,
DBC2-Powershell uses a malicious registry runkey for persistence which downloads and executes our published agent whenever a user logs on. The video below shows how the persistence technique works:

[![DBC2-Persistence](https://img.youtube.com/vi/-kUAJ4-rfdA/0.jpg)](http://www.youtube.com/watch?v=-kUAJ4-rfdA)

# Future Work

There is still a lot to do in this project.Due to my limited Python knowledge, I modified the controller code to suit my needs. For example manually setting the master key and iv in the pollingThread file instead of the initial default config file to better understand how the crypto works. 

There are also some features I'm yet to implement which are working well in the original DBC2 and they include fixing the shellmode which is functional but not yet ready to be fully demo’d, changing the polling time, setting the sleep time, better output from the custom PS modules. 
It's definitely on the to-do list :). I'll be devoting a few hours a week so that I can finally complete it. Any help is welcome.

# Conclusion 

Command and control over DropBox has awesome practical advantages. If you doubt the power of such a technique you only need to read the awesome report by Cyber-X code named [Operation BugDrop](https://cyberx-labs.com/en/blog/operation-bugdrop-cyberx-discovers-large-scale-cyber-reconnaissance-operation/)
That report only fueled me more to release an early version of this proof of concept even if incomplete.
The project can be found at [Invoke-DBC2.](https://github.com/Truneski/Invoke-DBC2)
Penetration Testers can now use DBC2-Powershell alongside familiar PowerShell tools.


