Download Link: https://assignmentchef.com/product/solved-comp9044assignment-1-shrug-shrug
<br>



This assignment aims to give you practice in Shell programming generally a clear concrete understanding of Git’s core semantics

Note: the material in the lecture notes will not be sufficient by itself to allow you to complete this assignment. You may need to search on-line documentation for Shell, Git, etc. Being able to search documentation efficiently for the information you need is a <em>very </em>useful skill for any kind of computing work. Introduction

Your task in this assignment is to implement Shrug, a subset of the version control system <a href="https://en.wikipedia.org/wiki/Git">Git</a><a href="https://en.wikipedia.org/wiki/Git">.</a>

Git is a <em>very</em> complex program which has many individual commands. You will implement only a few of the most important commands. You will also be given a number of simplifying assumptions, which make your task easier.

Shrug is a contraction of Shell-running-git.

You must implement Shrug in Shell.

Interestingly, early versions of Git made heavy use of Shell and Perl. Reference implementation

Many aspects of this assignment are not fully specified in this document; instead, you must match the behaviour of a reference implementation.

For example, your script shrug-add should match the behaviour of 2041 shrug-add exactly, including producing the same error messages.

Provision of a reference implementation is a common method to provide or define an operational specification, and it’s something you will likely need to do after you leave UNSW.

Discovering and matching the reference implementation’s behaviour is deliberately part of the assignment.

While the code in the reference implementation is fairly straight forward, reverse-engineering its behaviour is obviously not so simple, and is a nice example of how coming to grips with the precise semantics of an apparently obvious task can still be challenging.

If you discover what you believe to be a bug in the reference implementation, report it in the class forum. Andrew may fix the bug, or indicate that you do not need to match the reference implementation’s behaviour in this case.

<h1>Shrug Commands</h1>

<h2>Subset 0</h2>

Subset 0 commands must be implemented in POSIX-compatible Shell. See the Permitted Languages section for more information. shrug-init

The shrug-init command creates an empty Shrug repository.

shrug-init should create a directory named .shrug, which it will use to store the repository. It should produce an error message if this directory already exists.

You should match this, and other error messages exactly. For example:

<table width="961">

 <tbody>

  <tr>

   <td width="961">$ <strong>ls -d .shrug</strong> ls: cannot access ‘.shrug’: No such file or directory$ <strong>shrug-init</strong>Initialized empty shrug repository in .shrug$ <strong>ls -d .shrug</strong>.shrug $ <strong>shrug-init</strong> shrug-init: error: .shrug already exists</td>

  </tr>

 </tbody>

</table>

shrug-init may create initial files or directories inside .shrug.

You do not have to use a particular representation to store the repository.

You do not have to create the same files or directories inside shrug-init as the reference implementation. shrug-add <em>filenames…</em>

The shrug-add command adds the contents of one or more files to the index .

Files are added to the repository in a two step process. The first step is adding them to the index.

You will need to store files in the index somehow in the .shrug sub-directory. For example, you might choose store them in a subdirectory of .shrug.

Only ordinary files in the current directory can be added. You can assume filenames start with an alphanumeric character ( a-zA-Z09 ) and will only contain alpha-numeric characters, plus ‘.’, ‘-‘ and ‘_’ characters.

The shrug-add command, and other Shrug commands, will not be given pathnames with slashes. shrug-commit -m <em>message</em>

The shrug-commit command saves a copy of all files in the index to the repository.

A message describing the commit must be included as part of the commit command.

Shrug commits are numbered sequentially: they are not hashes, like Git. You must match the numbering scheme.

You can assume the commit message is ASCII, does not contain new-line characters, and does not start with a ‘-‘ character. shrug-log

The shrug-log command prints a line for every commit made to the repository: each line should contain the commit number, and the commit message.

<h3>shrug-show commit :filename</h3>

The shrug-show should print the contents of the specified <em>filename</em> as of the specified <em>commit</em>. If <em>commit</em> is omitted, the contents of the file in the index should be printed.

You can assume the commit if specified will be a non-negative integer.

For example:

<table width="961">

 <tbody>

  <tr>

   <td width="961">$ <strong>./shrug-init</strong>Initialized empty shrug repository in .shrug$ <strong>echo line 1 &gt; a</strong>$ <strong>echo hello world &gt;b</strong>$ <strong>./shrug-add a b</strong>$ <strong>./shrug-commit -m ‘first commit’</strong>Committed as commit 0$ <strong>echo  line 2 &gt;&gt;a</strong>$ <strong>./shrug-add a</strong>$ <strong>./shrug-commit -m ‘second commit’</strong>Committed as commit 1$ <strong>./shrug-log</strong>1 second commit0 first commit$ <strong>echo line 3 &gt;&gt;a </strong>$ <strong>./shrug-add a</strong>$ <strong>echo line 4 &gt;&gt;a </strong>$ <strong>./shrug-show 0:a</strong> line 1$ <strong>./shrug-show 1:a</strong> line 1</td>

  </tr>

 </tbody>

</table>

<h2>Subset 1</h2>

Subset 1 is more difficult. You will need spend some time understanding the semantics (meaning) of these operations, by running the reference implementation, or researching the equivalent Git operations.

Note the assessment scheme recognises this difficulty.

Subset 1 commands must be implemented in POSIX-compatible Shell. See the Permitted Languages section for more information. shrug-commit <em>-a</em> -m <em>message</em>

shrug-commit can have a -a option, which causes all files already in the index to have their contents from the current directory added to the index before the commit. shrug-rm <em>–force</em> <em>–cached</em> <em>filenames…</em>

shrug-rm removes a file from the index, or from the current directory and the index.

If the –cached option is specified, the file is removed only from the index, and not from the current directory.

shrug-rm, like git rm, should stop the user accidentally losing work, and should give an error message instead if the removal would cause the user to lose work. You will need to experiment with the reference implementation to discover these error messages. Researching git rm’s behaviour may also help.

The –force option overrides this, and will carry out the removal even if the user will lose work. shrug-status

shrug-status shows the status of files in the current directory, the index, and the repository.

$ <strong>./shrug-init</strong>

Initialized empty shrug repository in .shrug

$ <strong>touch a b c d e f g h</strong>

$ <strong>./shrug-add a b c d e f</strong>

$ <strong>./shrug-commit -m ‘first commit’</strong>

Committed as commit 0

$ <strong>echo hello &gt;a</strong>

$ <strong>echo hello &gt;b</strong>

$ <strong>echo hello &gt;c</strong>

$ <strong>./shrug-add a b</strong>

$ <strong>echo world &gt;a</strong>

$ <strong>rm d</strong>

$ <strong>./shrug-rm e</strong>

$ <strong>./shrug-add g </strong>$ <strong>./shrug-status</strong> a – file changed, different changes staged for commit b – file changed, changes staged for commit c – file changed, changes not staged for commit d – file deleted e – deleted f – same as repo

<h2>Subset 2</h2>

Subset 2 is extremely difficult. You will need spend considerable time understanding the semantics of these operations, by running the reference implementation, and/or researching the equivalent Git operations.

Note the assessment scheme recognises this difficulty.

Subset 2 commands must be implemented in POSIX-compatible Shell. See the Permitted Languages section for more information. shrug-branch <em>-d</em> <em>branch-name</em>

shrug-branch either creates a branch, deletes a branch, or lists current branch names. shrug-checkout <em>branch-name</em>

shrug-checkout switches branches.

Note that, unlike Git, you can not specify a commit or a file: you can only specify a branch. shrug-merge <em>branch-name</em>|<em>commit</em> -m <em>message</em>

shrug-merge adds the changes that have been made to the specified branch or commit to the index, and commits them.

<table width="961">

 <tbody>

  <tr>

   <td width="961">$ <strong>./shrug-init</strong>Initialized empty shrug repository in .shrug$ <strong>seq 1 7 &gt;7.txt</strong>$ <strong>./shrug-add 7.txt</strong>$ <strong>./shrug-commit -m commit-1</strong>Committed as commit 0 $ <strong>./shrug-branch b1</strong>$ <strong>./shrug-checkout b1</strong>Switched to branch ‘b1’$ <strong>perl -pi -e ‘s/2/42/’ 7.txt</strong>$ <strong>cat 7.txt</strong>14234567$ <strong>./shrug-commit -a -m commit-2</strong>Committed as commit 1$ <strong>./shrug-checkout master</strong></td>

  </tr>

 </tbody>

</table>

If a file contains conflicting changes, shrug-merge produces an error message.

<table width="961">

 <tbody>

  <tr>

   <td width="961">$ <strong>./shrug-init</strong>Initialized empty shrug repository in .shrug$ <strong>seq 1 7 &gt;7.txt</strong>$ <strong>./shrug-add 7.txt</strong>$ <strong>./shrug-commit -m commit-1</strong>Committed as commit 0 $ <strong>./shrug-branch b1</strong>$ <strong>./shrug-checkout b1</strong>Switched to branch ‘b1’$ <strong>perl -pi -e ‘s/2/42/’ 7.txt</strong>$ <strong>cat 7.txt</strong>14234567$ <strong>./shrug-commit -a -m commit-2</strong>Committed as commit 1$ <strong>./shrug-checkout master</strong></td>

  </tr>

 </tbody>

</table>

<h1>Diary</h1>

You must keep notes on each piece of work you make on this assignment. The notes should include date, starting and finishing time, and a brief description of the work carried out. For example:

<table width="961">

 <tbody>

  <tr>

   <td width="136">Date</td>

   <td width="103">Start</td>

   <td width="98">Stop</td>

   <td width="154">Activity</td>

   <td width="470">Comments</td>

  </tr>

  <tr>

   <td width="136">19/06/19</td>

   <td width="103">16 00</td>

   <td width="98">17 30</td>

   <td width="154">coding</td>

   <td width="470">implemented basic commit functionality</td>

  </tr>

  <tr>

   <td width="136">20/06/19</td>

   <td width="103">20 00</td>

   <td width="98">10 30</td>

   <td width="154">debugging</td>

   <td width="470">found bug in command-line arguments</td>

  </tr>

 </tbody>

</table>

Include these notes in the files you submit as an ASCII file named diary.txt.

<h1>Testing</h1>

<h2>Autotests</h2>

As usual, some autotests will be available:

$ <strong>2041 autotest shrug shrug-*</strong> …

You can also run only tests for a particular subset or an individual test:

$ <strong>2041 autotest shrug subset1 shrug-*</strong> …

$ <strong>2041 autotest shrug subset1_13 shrug-*</strong> …

If you are using extra Shell files, include them on the autotest command line.

Autotest and automarking will run your scripts with a current working directory different to the directory containing the script. The directory containing your submission will be in $PATH.

You will need to do most of the testing yourself.

<h2>Test Scripts</h2>

You should submit ten Shell scripts, named test00.sh to test09.sh, which run shrug commands that test an aspect of Shrug.

The test??.sh scripts do not have to be examples that your program implements successfully.

You may share your test examples with your friends, but the ones you submit must be your own creation.

The test scripts should show how you’ve thought about testing carefully. Permitted Languages

Your programs must be written entirely in POSIX-compatible shell.

Your programs will be run with <a href="https://manpages.debian.org/jump?q=dash.1"><em>dash</em></a><a href="https://manpages.debian.org/jump?q=dash.1"><em>(1)</em></a>, in /bin/dash. You can assume anything that works with the version of /bin/dash on CSE systems is POSIX compatible.

Start your programs with:

#!/bin/dash

If you want to run these scripts on your own machine — for example, one running macOS — which has <a href="https://manpages.debian.org/jump?q=dash.1"><em>dash</em></a><a href="https://manpages.debian.org/jump?q=dash.1"><em>(1)</em></a> installed somewhere other than /bin, use:

#!/usr/bin/env dash

You are permitted to use any feature /bin/dash provides.

On CSE systems, /bin/sh is the <u>Bash (Bourne-again shell)</u> shell: /bin/sh is a symlink to /bin/bash. Bash implements many nonPOSIX extensions, including regular expressions and arrays. These <em>will not work</em> with /bin/dash, and you are not permitted to use these for the assignment.

You are not permitted to use Perl, Python or any language other than POSIX-compatible shell.

You are permitted to use only these external programs:

<table width="877">

 <tbody>

  <tr>

   <td width="163"><a href="https://manpages.debian.org/jump?q=basename.1"><em>basename</em></a><a href="https://manpages.debian.org/jump?q=basename.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=bunzip2.1"><em>bunz</em></a><a href="https://manpages.debian.org/jump?q=bunzip2.1"><em>i</em></a><a href="https://manpages.debian.org/jump?q=bunzip2.1"><em>p</em></a><a href="https://manpages.debian.org/jump?q=bunzip2.1"><em>2(1) </em></a><a href="https://manpages.debian.org/jump?q=bzcat.1"><em>bzcat</em></a><a href="https://manpages.debian.org/jump?q=bzcat.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=bzip2.1"><em>bz</em></a><a href="https://manpages.debian.org/jump?q=bzip2.1"><em>i</em></a><a href="https://manpages.debian.org/jump?q=bzip2.1"><em>p</em></a><a href="https://manpages.debian.org/jump?q=bzip2.1"><em>2(1) </em></a><a href="https://manpages.debian.org/jump?q=cat.1"><em>cat</em></a><a href="https://manpages.debian.org/jump?q=cat.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=chmod.1"><em>chm</em></a><a href="https://manpages.debian.org/jump?q=chmod.1"><em>o</em></a><a href="https://manpages.debian.org/jump?q=chmod.1"><em>d</em></a><a href="https://manpages.debian.org/jump?q=chmod.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=cmp.1"><em>cmp</em></a><a href="https://manpages.debian.org/jump?q=cmp.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=cp.1"><em>cp</em></a><a href="https://manpages.debian.org/jump?q=cp.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=cpio.1"><em>cpi</em></a><a href="https://manpages.debian.org/jump?q=cpio.1"><em>o(1) </em></a><a href="https://manpages.debian.org/jump?q=csplit.1"><em>csplit</em></a><a href="https://manpages.debian.org/jump?q=csplit.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=cut.1"><em>cut</em></a><a href="https://manpages.debian.org/jump?q=cut.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=date.1"><em>date</em></a><a href="https://manpages.debian.org/jump?q=date.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=dc.1"><em>dc</em></a><a href="https://manpages.debian.org/jump?q=dc.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=dd.1"><em>dd</em></a><a href="https://manpages.debian.org/jump?q=dd.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=df.1"><em>df</em></a><a href="https://manpages.debian.org/jump?q=df.1"><em>(1)</em></a></td>

   <td width="163"><a href="https://manpages.debian.org/jump?q=diff.1"><em>diff</em></a><a href="https://manpages.debian.org/jump?q=diff.1"><em>(1)</em></a><a href="https://manpages.debian.org/jump?q=dirname.1"><em>dirname</em></a><a href="https://manpages.debian.org/jump?q=dirname.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=du.1"><em>du</em></a><a href="https://manpages.debian.org/jump?q=du.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=echo.1"><em>ech</em></a><a href="https://manpages.debian.org/jump?q=echo.1"><em>o(1) </em></a><a href="https://manpages.debian.org/jump?q=egrep.1"><em>e</em></a><a href="https://manpages.debian.org/jump?q=egrep.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=egrep.1"><em>rep</em></a><a href="https://manpages.debian.org/jump?q=egrep.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=env.1"><em>env</em></a><a href="https://manpages.debian.org/jump?q=env.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=expand.1"><em>expand</em></a><a href="https://manpages.debian.org/jump?q=expand.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=expr.1"><em>expr</em></a><a href="https://manpages.debian.org/jump?q=expr.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=false.1"><em>false</em></a><a href="https://manpages.debian.org/jump?q=false.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=fgrep.1"><em>f</em></a><a href="https://manpages.debian.org/jump?q=fgrep.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=fgrep.1"><em>rep</em></a><a href="https://manpages.debian.org/jump?q=fgrep.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=find.1"><em>find</em></a><a href="https://manpages.debian.org/jump?q=find.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=fold.1"><em>f</em></a><a href="https://manpages.debian.org/jump?q=fold.1"><em>o</em></a><a href="https://manpages.debian.org/jump?q=fold.1"><em>ld</em></a><a href="https://manpages.debian.org/jump?q=fold.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=getopt.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=getopt.1"><em>etopt</em></a><a href="https://manpages.debian.org/jump?q=getopt.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=grep.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=grep.1"><em>rep</em></a><a href="https://manpages.debian.org/jump?q=grep.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=gunzip.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=gunzip.1"><em>unzip</em></a><a href="https://manpages.debian.org/jump?q=gunzip.1"><em>(1)</em></a></td>

   <td width="163"><a href="https://manpages.debian.org/jump?q=gzip.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=gzip.1"><em>zip</em></a><a href="https://manpages.debian.org/jump?q=gzip.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=head.1"><em>head</em></a><a href="https://manpages.debian.org/jump?q=head.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=hostname.1"><em>hostname</em></a><a href="https://manpages.debian.org/jump?q=hostname.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=less.1"><em>less</em></a><a href="https://manpages.debian.org/jump?q=less.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=ln.1"><em>ln</em></a><a href="https://manpages.debian.org/jump?q=ln.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=ls.1"><em>ls</em></a><a href="https://manpages.debian.org/jump?q=ls.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=lzcat.1"><em>lzcat</em></a><a href="https://manpages.debian.org/jump?q=lzcat.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=lzma.1"><em>lzma</em></a><a href="https://manpages.debian.org/jump?q=lzma.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=md5sum.1"><em>md</em></a><a href="https://manpages.debian.org/jump?q=md5sum.1"><em>5</em></a><a href="https://manpages.debian.org/jump?q=md5sum.1"><em>sum</em></a><a href="https://manpages.debian.org/jump?q=md5sum.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=mkdir.1"><em>mkdir</em></a><a href="https://manpages.debian.org/jump?q=mkdir.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=mktemp.1"><em>mktemp</em></a><a href="https://manpages.debian.org/jump?q=mktemp.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=more.1"><em>more</em></a><a href="https://manpages.debian.org/jump?q=more.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=mv.1"><em>mv</em></a><a href="https://manpages.debian.org/jump?q=mv.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=patch.1"><em>patch</em></a><a href="https://manpages.debian.org/jump?q=patch.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=printf.1"><em>printf</em></a><a href="https://manpages.debian.org/jump?q=printf.1"><em>(1)</em></a></td>

   <td width="145"><a href="https://manpages.debian.org/jump?q=pwd.1"><em>pwd</em></a><a href="https://manpages.debian.org/jump?q=pwd.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=readlink.1"><em>readlink</em></a><a href="https://manpages.debian.org/jump?q=readlink.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=realpath.1"><em>rea</em></a><a href="https://manpages.debian.org/jump?q=realpath.1"><em>l</em></a><a href="https://manpages.debian.org/jump?q=realpath.1"><em>path</em></a><a href="https://manpages.debian.org/jump?q=realpath.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=rev.1"><em>rev</em></a><a href="https://manpages.debian.org/jump?q=rev.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=rm.1"><em>rm</em></a><a href="https://manpages.debian.org/jump?q=rm.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=rmdir.1"><em>rmdir</em></a><a href="https://manpages.debian.org/jump?q=rmdir.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=sed.1"><em>sed</em></a><a href="https://manpages.debian.org/jump?q=sed.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=seq.1"><em>seq</em></a><a href="https://manpages.debian.org/jump?q=seq.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=sha1sum.1"><em>sha</em></a><a href="https://manpages.debian.org/jump?q=sha1sum.1"><em>1</em></a><a href="https://manpages.debian.org/jump?q=sha1sum.1"><em>sum</em></a><a href="https://manpages.debian.org/jump?q=sha1sum.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=sha256sum.1"><em>sha</em></a><a href="https://manpages.debian.org/jump?q=sha256sum.1"><em>256</em></a><a href="https://manpages.debian.org/jump?q=sha256sum.1"><em>sum</em></a><a href="https://manpages.debian.org/jump?q=sha256sum.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=sha512sum.1"><em>sha</em></a><a href="https://manpages.debian.org/jump?q=sha512sum.1"><em>512</em></a><a href="https://manpages.debian.org/jump?q=sha512sum.1"><em>sum</em></a><a href="https://manpages.debian.org/jump?q=sha512sum.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=sleep.1"><em>sleep</em></a><a href="https://manpages.debian.org/jump?q=sleep.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=sort.1"><em>sort</em></a><a href="https://manpages.debian.org/jump?q=sort.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=stat.1"><em>stat</em></a><a href="https://manpages.debian.org/jump?q=stat.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=strings.1"><em>strin</em></a><a href="https://manpages.debian.org/jump?q=strings.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=strings.1"><em>s</em></a><a href="https://manpages.debian.org/jump?q=strings.1"><em>(1)</em></a></td>

   <td width="178"><a href="https://manpages.debian.org/jump?q=tac.1"><em>tac</em></a><a href="https://manpages.debian.org/jump?q=tac.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=tail.1"><em>tail</em></a><a href="https://manpages.debian.org/jump?q=tail.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=tar.1"><em>tar</em></a><a href="https://manpages.debian.org/jump?q=tar.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=tee.1"><em>tee</em></a><a href="https://manpages.debian.org/jump?q=tee.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=test.1"><em>test</em></a><a href="https://manpages.debian.org/jump?q=test.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=time.1"><em>time</em></a><a href="https://manpages.debian.org/jump?q=time.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=top.1"><em>top</em></a><a href="https://manpages.debian.org/jump?q=top.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=touch.1"><em>touch</em></a><a href="https://manpages.debian.org/jump?q=touch.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=tr.1"><em>tr</em></a><a href="https://manpages.debian.org/jump?q=tr.1"><em>(1)</em></a><a href="https://manpages.debian.org/jump?q=true.1"><em>true</em></a><a href="https://manpages.debian.org/jump?q=true.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=uname.1"><em>uname</em></a><a href="https://manpages.debian.org/jump?q=uname.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=uncompress.1"><em>unc</em></a><a href="https://manpages.debian.org/jump?q=uncompress.1"><em>o</em></a><a href="https://manpages.debian.org/jump?q=uncompress.1"><em>mpress</em></a><a href="https://manpages.debian.org/jump?q=uncompress.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=unexpand.1"><em>unexpand</em></a><a href="https://manpages.debian.org/jump?q=unexpand.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=uniq.1"><em>uniq</em></a><a href="https://manpages.debian.org/jump?q=uniq.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=unlzma.1"><em>unlzma</em></a><a href="https://manpages.debian.org/jump?q=unlzma.1"><em>(1)</em></a></td>

   <td width="65"><a href="https://manpages.debian.org/jump?q=unxz.1"><em>unxz</em></a><a href="https://manpages.debian.org/jump?q=unxz.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=unzip.1"><em>unzip</em></a><a href="https://manpages.debian.org/jump?q=unzip.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=wc.1"><em>wc</em></a><a href="https://manpages.debian.org/jump?q=wc.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=wget.1"><em>w</em></a><a href="https://manpages.debian.org/jump?q=wget.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=wget.1"><em>et</em></a><a href="https://manpages.debian.org/jump?q=wget.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=which.1"><em>which</em></a><a href="https://manpages.debian.org/jump?q=which.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=who.1"><em>wh</em></a><a href="https://manpages.debian.org/jump?q=who.1"><em>o(1) </em></a><a href="https://manpages.debian.org/jump?q=xargs.1"><em>xar</em></a><a href="https://manpages.debian.org/jump?q=xargs.1"><em>g</em></a><a href="https://manpages.debian.org/jump?q=xargs.1"><em>s</em></a><a href="https://manpages.debian.org/jump?q=xargs.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=xz.1"><em>xz</em></a><a href="https://manpages.debian.org/jump?q=xz.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=xzcat.1"><em>xzcat</em></a><a href="https://manpages.debian.org/jump?q=xzcat.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=yes.1"><em>y</em></a><a href="https://manpages.debian.org/jump?q=yes.1"><em>es</em></a><a href="https://manpages.debian.org/jump?q=yes.1"><em>(1) </em></a><a href="https://manpages.debian.org/jump?q=zcat.1"><em>zcat</em></a><a href="https://manpages.debian.org/jump?q=zcat.1"><em>(1)</em></a></td>

  </tr>

 </tbody>

</table>

Only a few of the programs in the above list are likely to be useful for the assignment.

Note you are permitted to use built-in shell features including: cd, exit, for, if, read, shift and while.

If you wish to use an external program which is not in the above list, please ask in the class forum for it to be added.

You may submit extra Shell files. Assumptions/Clarifications

Like all good programmers, you should make as few assumptions as possible.

You can assume shrug commands are always run in the same directory as the repository, and only files from that directory are added to the repository.

You can assume the directory in which shrug commands are run will not contain sub-directories apart from .shrug.

You can assume where a branch name is expected a string will be supplied starting with an alphanumeric character ( a-zA-Z0-9 ), and only containing alphanumeric characters plus ‘-‘ and ‘_’. In additon a branch name will not be supplied which is entirely numeric. This allows brnach names to be distinguished from commits when merging.

You can assume where a filenames is expected a string will be supplied starting with an alphanumeric character ( a-zA-Z0-9 ) and only containing alpha-numeric characters, plus ‘.’, ‘-‘ and ‘_’ characters.

You can assume where a commit number is expected a string will be supplied which is a non-negative integer with no leading zeros. It will not contain white space or any other charcters except digits.

You can assume that shrug-add, shrug-show, and shrug-rm will be given just a filename, not pathnames with slashes.

You do not have to consider file permissions or other file metadata. For example, you do not have to ensure files created by a checkout command have the same permissions as when they were added.

You do not have to handle concurrency. You can assume only one instance of any shrug command is running at any time.

You can assume that only the arguments described above are supplied to shrug commands. You do not have to handle other arguments.

You should match the output streams used by the reference implementations. It writes error messages to stderr: so should you.

You should match the exit status used by the reference implementation. It exits with status 1 after an error: so should you.

Autotest and automarking will run your scripts with a current working directory different to the directory containing the script. This may break Shell with code in extra files: if so, ask for help in the forum. The directory containing your submission will be in $PATH.

You can assume arguments will be in the position and order shown in the usage message from the reference implementation. Other orders and positions will not be tested. For example, here is the usage message for shrug-rm:

$ <strong>2041 shrug-rm</strong>

usage: shrug-rm [–force] [–cached] &lt;filenames&gt;

So, you assume that if the –force or –cached options are present, they come before all filenames, and if they are both present the –force option will come first. Change Log

Version 0.1                                                     Initial release

(2020-06-26 14 00)

Version 0.2                                                            bug in reference implementation fixed for shrug-status when file changed or removed after

(2020-07-06 21 00)                                                                          being added

Version 0.3                                                           bug in reference implementation fixed for shrug-show for file in index before first commit

(2020-07-06 16 00)

Version 0.4                                                            add clarification about what can be supplied as arguments – fix minor bug in reference

(2020-07-06 16 00)                                                                              implementation for certain commit messages

When you think your program is working, you can use autotest to run some simple automated tests:

$ <strong>2041 autotest shrug</strong>

<h2></h2>