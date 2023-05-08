Download Link: https://assignmentchef.com/product/solved-solvedassignment-5-ipc-pipes
<br>
GoalAlways use gcc -m32 -Wall (source code) to seek and eliminate any compiler warnings from now on. The “-Wall” option means “warn-all” possible errors. This will make good programming practice. From this assignment on, compiler warnings will cause points deduction during assignment grading.

The given doit shell script is a tool you should use (and modify it to your own likings). Adding a source-file name (without extension “.c”) and it will compile and name the executable accordingly; e.g., doit parent will use source code “parent.c” to compile into executable “parent.”

Write a C programs parent.c and child.c which performs the same game-play as described in the previous assignment except that: the parent will be the one that distributes random values to child processes through “pipes,” an Inter-Process Communication method.

All child processes will know where to read from the same pipe to retrieve numbers. There must be some synchronization in order to avoid “racing” conditions, i.e., whose number is it now in the pipe or whose turn is it this time to read it to sum?

A simple way for to distribute numbers via a single pipe to different destinations: after placing the number in, send a signal to the one that has the current turn. The child will then read the number and sum via its own already-registered signal handler. Thus, the same pipe is shared in the correct round-robin order.

And, whenever a child reaches the goal, it exits with the final sum. The parent, in turns, will catch this event via its ownt signal handler that handles the event of a child exiting. The parent can than show the result and tells everyone to quit (including itself).DetailsThe parent will first request a pipe from the OS. A pipe is actually like a file/data buffer with two ends: a reading end to read data from, and a writing end to write data into. System calls read and write we used before (to read keyboard and display) also apply to these two ends (of course, in different file numbers rather the stdin and stdout).

Writing to a pipe does not block the process. Reading, however, may (similar to reading from the keyboard when nothing typed), if there aren’t data in the pipe. Not enough data for the number of bytes requested in the read() call does not block, instead, the return number of the call will be the number of bytes actually read.

A child, once fork-execl-ed, will have the reading end of the pipe (in its arguments). The parent uses the writing end of the pipe. This is a one-way pipe.

Signals are still be needed in order for a child to read the pipe at the right time after the parent writes a number into the pipe and signals it. A child once reaching the goal number will exit with the sum (for the minimum assignment requirement). The exit event serves as the signal for the parent to get its result, and stop everyone from continuing (including itself).ChallengesReading a pipe may block the process if there’re no data at all. (Not enough data to match the size of the read request doesn’t block, check the return number from the read() call for the number of bytes actually read). A pipe buffers data until being read. A pipe may get too full or become broken — signal SIGPIPE.

Can using multiple pipes help the parent to distribute numbers without signaling? That is, if each child is given a private pipe to read? Can the play turns still be enforced fairly? (They still use exit calls to return sums.)

Can using two-way pipes help? This means the parent will prepare two pipes for each child. The child will write back the updated sum of its round to the parent which will read and thus synchronize the play turn. (The parent may be blocked until the child finishes its current turn, to proceed to the next child.)

The parent can record the number of rounds and the final sum as each child reaches the goal, and can thus continue to serve the rest until all have finished.

The parent shows all results after all children have done playing. What is to be show are the child PID and its sum — in the order of the original winning sequence during the play loop. (1st-win-1st-recorded, see the extra-credit demo.)

As said, reading a pipe may block (when there aren’t any data), and this can actually enforce the orderly round-robin fair play since the parent will wait for the currently-playing child to write back after playing its turn, before serving the next child.

Extra credit (2 points) will be given if no signals are needed when using two-way pipes as stated above.OS Service CallsThe calls that are allowed to use in code are listed below (signal calls allowed for minimum/non-extra credit requirement):pipe() to request for pipesread() to read from a pipewrite() to write to a pipeclose() to close a pipeprintf() allowed only to show running statusatoi() to convert argument strings to numberssprintf() to build strings to pass as argumentssrand() to seed randomizationrand() to get a random numberexit() (child exits quietly in extra-credit version)wait() (extra-credit part can do without, but an etiquette to receive deaths)Turn-inTurn in your source code the same way as you were required to do for other programming assignments. E-mailing will not be accepted, no late turn-ins.

Find folder “5” in the folder of your name, to put your source-code files (only): parent.c and child.c.

Write code with a clean format, preamble/header, and put comments at places that would help with grading (your reasoning).