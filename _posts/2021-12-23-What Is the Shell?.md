---

layout: post
title: What is the shell?
date: 2021-12-23 21:35:00
author: "phjung1"
header-img: "#"
tags:

- linux
- shell

---

# What Is the Shell?



When we speak of the command line, we are really referring to the shell. 

The shell is a program that takes keyboard commands and passes them to the operating system to carry out. 

Almost all Linux distributions supply a shell program from the GNU Project called bash. 

The name is an acronym for bourne-again shell, a reference to the fact that bash is an enhanced replacement for sh, the original Unix shell program written by Steve Bourne.



## Terminal Emulators

When using a graphical user interface (GUI), we need another program called a terminal emulator to interact with the shell.

 If we look through our desktop menus, we will probably find one. KDE uses konsole, and GNOME uses gnome-terminal, though it’s likely called simply Terminal on your menu. 

A number of other terminal emulators are available for Linux, but they all basically do the same thing: give us access to the shell. You will probably develop a preference for one or another terminal emulator based on the number of bells and whistles it has.



## Making Your First Keystrokes

So let’s get started. Launch the terminal emulator. Once it comes up, we
should see something like this:

    [me@linuxbox ~]$

This is called a shell prompt, and it will appear whenever the shell is
ready to accept input. While it might vary in appearance somewhat depending on the distribution, it will typically include your username@machinename,
followed by the current working directory (more about that in a little bit)
and a dollar sign.



If the last character of the prompt is a hash mark (#) rather than a
dollar sign, the terminal session has superuser privileges. This means either
we are logged in as the root user or we selected a terminal emulator that
provides superuser (administrative) privileges



Assuming things are good so far, let’s try some typing. Enter some
gibberish at the prompt like so:



    [me@linuxbox ~]$ kaekfjaeifj

Because this command makes no sense, the shell tells us so and gives us
another chance.



    bash: kaekfjaeifj: command not found
    [me@linuxbox ~]$



## Command History

If we press the up arrow, we will see that the previous command entered,
kaekfjaeifj, reappears after the prompt. This is called command history. Most
Linux distributions remember the last 1,000 commands by default. Press
the down arrow and the previous command disappears.



## Cursor Movement

Recall the previous command by pressing the up arrow again. If we try the
left and right arrows, we’ll see that we can position the cursor anywhere on
the command line. This makes editing commands easy.



## Try Some Simple Commands

Now that we have learned to enter text in the terminal emulator, let’s try a
few simple commands. Let’s begin with the date command, which displays
the current time and date.

    [me@linuxbox ~]$ date
    Fri Feb 2 15:09:41 EST 2018

A related command is cal, which, by default, displays a calendar of the
current month.

    [me@linuxbox ~]$ cal
     February 2018
    Su Mo Tu We Th Fr Sa
     1 2 3
     4 5 6 7 8 9 10
    11 12 13 14 15 16 17
    18 19 20 21 22 23 24
    25 26 27 28 



To see the current amount of free space on our disk drives, enter df

    [me@linuxbox ~]$ df
    Filesystem 1K-blocks Used Available Use% Mounted on
    /dev/sda2 15115452 5012392 9949716 34% /
    /dev/sda5 59631908 26545424 30008432 47% /home
    /dev/sda1 147764 17370 122765 13% /boot
    tmpfs 256856 0 256856 0% /dev/shm



Likewise, to display the amount of free memory, enter the free command.

    [me@linuxbox ~]$ free
     total used free shared buffers cached
    Mem: 513712 503976 9736 0 5312 122916
    -/+ buffers/cache: 375748 137964
    Swap: 1052248 104712 947536



## Ending a Terminal Session

We can end a terminal session by closing the terminal emulator window, by
entering the exit command at the shell prompt, or by pressing ctrl-D.

    [me@linuxbox ~]$ exit



## Summing Up

This chapter marked the beginning of our journey into the Linux command line, with an introduction to the shell, a glimpse of the command
line, and a brief lesson on how to start and end a terminal session. We also
saw how to issue some simple commands and perform a little light commandline editing. That wasn’t so scary, was it?



In the next chapter, we’ll learn a few more commands and wander
around the Linux file system.


