Download Link: https://assignmentchef.com/product/solved-cs1550-part-1-setting-up-the-xv6-development-environment
<br>
PART 1: SETTING UP THE XV6 DEVELOPMENT ENVIRONMENT.

We will use the Linux cluster provided by the CS Department.

<ol>

 <li>First, whenever the amount of AFS free space in your account is less than 500MB, you can carry out the following process to increase. your disk quota (thanks to our Tech Support Team!):

  <ol>

   <li>Log in to https://my.pitt.edu</li>

   <li>Click on “Profile” at the top of the screen</li>

   <li>Click on “Manage Your Account”</li>

   <li>Click on “Manage Email Quota”</li>

   <li>Click on “Increase My UNIX Quota”</li>

  </ol></li>

 <li>Log in to linux.cs.pitt.edu using your Pitt account. From a UNIX box, you can type: <strong>ssh <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3e554d53090d7e5257504b46105d4d104e574a4a105b5a4b">[email protected]</a></strong> assuming ksm73 is your Pitt ID. From Windows, you may use PuTTY.</li>

 <li>Under Linux, run the following command to download the Xv6 source code: <strong>git clone git://github.com/mit-pdos/xv6-public.git</strong></li>

 <li><strong>cd xv6-public </strong>then type <strong>make qemu-nox</strong> to compile and run XV6. To exit, press CTRL+A then X.</li>

</ol>




PART 2: ADDING A SYSTEM CALL TO XV6<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>

You’ll then add a new system call called getcount to xv6, which, when passed a valid system call number (listed in the file “syscall.h”) as an argument, will return the number of times the referenced system call was invoked by the calling process.

For instance, consider the following test program (getcount.c):

#include “types.h”

#include “user.h” #include “syscall.h”

int  main(int argc, char *argv[])

{

printf(1, “initial fork count %d
”, getcount(SYS_fork));     if (fork() == 0) {

printf(1, “child fork count %d
”, getcount(SYS_fork));         printf(1, “child write count %d
”, getcount(SYS_write));

} else {         wait();

printf(1, “parent fork count %d
”, getcount(SYS_fork));         printf(1, “parent write count %d
”, getcount(SYS_write));

}

printf(1, “wait count %d
”, getcount(SYS_wait));     exit();

}

will produce the following output (note that each character is output with a separate call to write/printf in xv6):

initial fork count 0 child fork count 0 child write count 19 wait count 0 parent fork count 1 parent write count 41 wait count 1

<strong> </strong>

<strong>Hints </strong>

You will need to modify several files for this exercise, though the total number of lines of code you’ll be adding is quite small. At a minimum, you’ll need to alter syscall.h, syscall.c, user.h, and usys.S to implement your new system call.  It may be helpful to trace how some other system call is implemented (e.g., uptime) for clues.




2

You will likely also need to update struct proc, located in proc.h, to add a syscall-count tracking data structure for each process. To re-initialize your data structure when a process terminates, you may want to look into the functions in proc.c.

Chapter 3 of the <a href="https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev10.pdf">xv6 book</a> (in pdf) contains details on traps and system calls (though most of the low level details won’t be necessary for you to complete this exercise).

<strong>Testing </strong>

To test your implementation, you’ll run the getcount executable (when booted into xv6), which is based on the program above. Because the program depends on your implementation, it isn’t compiled into xv6 by default. When you’re ready to test, you should add  getcount to UPROGS declaration in the Makefile.

Note that while the getcount <em>example </em>only prints out counts for three different system calls, your implementation should support all the system calls listed in syscall.h. It is a good exercise to add tests to the test program, located in getcount.c, for other system calls. 3

<a href="#_ftnref1" name="_ftn1">[1]</a> Adapted from http://moss.cs.iit.edu/cs450/assign01-xv6-syscall.html