# CSE 15L Week 9 Lab Report: Past Lab Activity

By Benjamin Johnson

## Contents

### [Introduction](#introduction-1)

### [My Bash Script](#my-bash-script-1)

### [SSH Host Shortcuts](#ssh-host-shortcuts-1)

### [Bash Aliases](#bash-aliases-1)

### [Running Everything](#running-everything-1)

### [Conclusion](#conclusion-1)

## Introduction

For this CSE 15L lab report, our task is to write about a previous lab report or lab activity. I chose to write about CSE Labs Done Quick from Week 7 (my previous lab report). CSE Labs Done Quick was a competition to see who could finish a series of comamnd-line tasks the fastest. For a description of the tasks and how I solved them, see [my previous lab report](./week7)

The reason I'm writing this lab report about those same tasks is because I have a much faster way of finishing these tasks than what I described in my previous report. The only problem: it's against the rules. Apparently. My solution is to use a Bash script that contains the commands for all the steps, then just run that command from the command line, and it will run the commands for all the asks. It turns out Bash scripts are against the rules for the competition, despite this not being mentioned in the rules. So in lab when we were preparing for the competition, I came up with this really fast way of finishing the tasks, and was really proud of myself for it, but then I found out that wasn't allowed for the competition. Oh well.

Anyway, in this lab report, I'm showing my Bash script and how I ran it to complete the tasks so quickly, along with a couple other shortcuts (Bash aliases and SSH host shortcuts) that I used to make this process even faster, but that also apparently were against the rules.

## My Bash Script

These are the contents of the Bash script I wrote:

```bash
git clone git@github.com:benjaminJohnson2204/lab7.git
cd lab7
javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests

sed '43 s/index1/index2/' ListExamples.java -i

javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests

git add ListExamples.java
git commit -m "Fix list examples"
git push
```

The commands in the script are pretty much identical to the commands I described in my Week 7 lab report. The one thing to note is that I had to change the name of the test file in the `java` command to run the tests, from "TestListExamples" to "ListExamplesTests", because the CSE 15L course staff renamed the test file in the "lab7" repository between when I wrote my Week 7 lab report and when I'm writing this lab report.

Here's a quick overview of the commands in this file:

- Line 1 (the `git clone` command) clones the lab7 repository. - Line 2 (`cd lab7`) moves into the newly cloned lab7 directory. - Line 3 (`javac`) compiles the Java files and tests. - Line 4 (`java`) runs the tests (at this point they are supposed to fail since there is a bug with the code being tested). - Line 6 (`sed`) edits the buggy code to fix its bug, using `sed`, which edits a file in-place (modifies and saves it). See [my previous lab report](./week7) for a more detailed description of the syntax and the bug with the code, but this command essentially replaces "index1" with "index2" on line 43 of "ListExamples.java", in order to fix a bug with the index variables causing an infinite loop when merging two lists. - Lines 8 and 9 compile and run the tests again, same as lines 3 and 4. - Lines 11-13 add, commit, and push the changes to git.

I saved this Bash script in a file called `b.sh` on my ieng6 SSH account, because we had to complete these tasks from our SSH accounts. I gave the file a short name so it would be fast to type the command to run it: `bash b.sh`.

## SSH Host Shortcuts

In Week 7 lab, we learned how to set up SSH keys so that we don't have to type in our passwords for our ieng6 accounts when we SSH into them. A way to SSH even faster is to use SSH hosts, which are a list of hosts with host names, usernames, and aliases, which make it faster to run them.

To add an SSH host, you can create (or edit, if it already exists) a file called `config` in the `.ssh` directory of your home directory (the same directory where you add SSH key files). Here are the contents of my config file:

```
Host i
    HostName ieng6.ucsd.edu
    User cs15lwi23aak
```

The first line, "Host i", means that I am specifying a host named "i". With this configuaration, I can SSH into my CSE 15L ieng6 account using the command `ssh i`. The "HostName" line specifies the host (the address of the SSH server), and the "User" line specifies the username to SSH into on that host. In an SSH command, these properties are specified with `ssh <user>@<host>`

With this configuration, `ssh i` does the same thing as `ssh cs15lwi23aak@ieng6.ucsd.edu`. This means I can SSH into my account, the first timed task, much more quickly.

## Bash Aliases

Similar to SSH aliases, you can create Bash aliases: names for commands run a specific command when you type them. For example, if I create an alias called "l" that is aliased to the command "ls", then typing `l` in Bash will have the effect of running `ls`. An alias can be for multiple and/or very long commands, which can make it much easier and faster to type commands.

To create a Bash alias, you can edit the `.bash_profile` file in your home directory. `.bash_profile` contains a series of commands that will be run every time you open a Bash terminal, when Bash starts up. In this file, you can do things like setting environment variables and shortcuts, and then those variables and shortcuts will be available every time you open a Bash shell, because `.bash_profile` will be run and will set the variables and shortcuts.

I created 2 aliases: one on my local machine to SSH into ieng6, and one on my ieng6 account to run my Bash script described above. On my local machine, `.bash_profile` was empty, so I added the following line to it:

```bash
alias a='ssh i'
```

This runs an `alias` command, which creates an alias for "a" to run "ssh i" (which, because of my SSH hosts file, will SSH into my ieng6 account). So now in a Bash terminal I can just type `a` and it will SSH into ieng6. In retrospect, using the alias I didn't also need an SSH host alias, because I could have just specified the full `ssh cs15lwi23aak@ieng6.ucsd.edu` in my Bash alias, but I created the SSH alias before I thought to use Bash aliases.

On my ieng6 account, I created an alias to run my Bash script. The `~/.bash_profile` file on my account already created some configuration commands created by IT, so I added an alias command at the bottom:

```bash
[other stuff...]

alias b='bash b.sh'
```

This creates an alias "b" to run my Bash script with `bash b.sh`. So now I can complete all the tasks on ieng6 by literally just typing `b` into my Bash terminal.

## Running Everything

Now, the 7+ steps for CSE Labs Done Quick can be reduced to 2 steps:

1. From a local Bash terminal, type `a` and press enter.
2. When the SSH terminal loads (or buffer the command before, it doesn't really matter), type `b` and press enter.

The fastest I could get everything to run was around 10 seconds. I think it's limited by the speed of connecting to the SSH server and the Github server to clone and push.

Here is a video of me running these commands:

https://benjaminjohnson2204.github.io/cse15l-lab-reports/videos/week9/CSELabsDoneQuick.mp4

Here is the full input and output:

```
$ a
b
Last login: Wed Mar  8 12:28:03 2023 from 100.81.33.240
b
Hello cs15lwi23aak, you are currently logged into ieng6-201.ucsd.edu

You are using 0% CPU on this system

Cluster Status
Hostname     Time    #Users  Load  Averages
ieng6-201   12:25:01   14  0.27,  0.40,  0.40
ieng6-202   12:25:01   16  0.77,  0.68,  0.50
ieng6-203   12:25:01   15  0.41,  0.29,  0.40


        *** SDSC Core Network Upgrade ***
        2023-03-07 19:00 - 2023-03-08 03:00
This work is now complete. If there were any disruptions to your
work or notice any problems that may have been caused by the network
upgrades please report them to the service desk.

https://support.ucsd.edu/its



Wed Mar 08, 2023 12:29pm - Prepping cs15lwi23
[cs15lwi23aak@ieng6-201]:~:499$ b
Cloning into 'lab7'...
remote: Enumerating objects: 35, done.
remote: Total 35 (delta 0), reused 0 (delta 0), pack-reused 35
Receiving objects: 100% (35/35), 372.19 KiB | 1.48 MiB/s, done.
Resolving deltas: 100% (12/12), done.
JUnit version 4.13.2
..E
Time: 0.55
There was 1 failure:
1) testMerge2(ListExamplesTests)
org.junit.runners.model.TestTimedOutException: test timed out after 500 milliseconds
        at java.util.Arrays.copyOf(Arrays.java:3210)
        at java.util.Arrays.copyOf(Arrays.java:3181)
        at java.util.ArrayList.grow(ArrayList.java:267)
        at java.util.ArrayList.ensureExplicitCapacity(ArrayList.java:241)
        at java.util.ArrayList.ensureCapacityInternal(ArrayList.java:233)
        at java.util.ArrayList.add(ArrayList.java:464)
        at ListExamples.merge(ListExamples.java:42)
        at ListExamplesTests.testMerge2(ListExamplesTests.java:19)

FAILURES!!!
Tests run: 2,  Failures: 1

JUnit version 4.13.2
..
Time: 0.013

OK (2 tests)

[main 08cf37c] Fix list examples
 Committer: Benjamin C Johnson <cs15lwi23aak@ieng6-201.ucsd.edu>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 1 file changed, 1 insertion(+), 1 deletion(-)
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 366 bytes | 183.00 KiB/s, done.
Total 3 (delta 1), reused 1 (delta 1), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:benjaminJohnson2204/lab7.git
   f750e52..08cf37c  main -> main
```

As you can see, when I started the task, I immediately typed "a", followed by enter, followed by "b", followed by enter. "a" ran my alias to SSH into my ieng6 account, and once that terminal loaded, "b" ran my alias to run my Bash script that ran the tasks.

## Conclusion

I definitely learned a lot from coming up with the fastest possible way to solve these tasks. I learned about Bash scripts, aliases, and SSH host aliases. Even though spending hours developing the fastest possible way to run a series of the same commands isn't something I would be doing in the real world, scripts and alises can absolutely be helpful for doing similar repetitive commands more quickly.
