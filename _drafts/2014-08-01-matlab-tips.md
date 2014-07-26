---
layout: post
title: MATLAB tips
---

http://rucool.marine.rutgers.edu/manuals/uncategorized/making-your-life-easier-with-matlab-startup-m/

MATLAB R2014A on Mac OS X 10.9.4

##Changing the default MATLAB directory (Apple)

Suppose that you have your own directory for MATLAB programs and files that you create. When you start MATLAB, it most likely has the working directory set to something else. These instructions show how to change that.

In the MATLAB environment, you can find the working directory with the pwd command. Exit the MATLAB software before doing the steps below. Call up a terminal window to enter the commands below.

You have to change "startup.m" file. If it does not exist, first copy it from "startupsav.m". First, change to the local MATLAB directory (the directory may be different for computers other than Apple). The commands in blue are what you type; the green text is the system's response. Text in red indicates that you will probably need to change it for your computer. (Note that "ibook" and "carmaux" are the names of my computers; these will likely be different for you, too.)

Change to the MATLAB directory:
ibook$ cd /Applications/MATLAB7/toolbox/local
Check to see if the file exists.
ibook$ ls startup.m
ls: startup.m: No such file or directory
If it does exist, skip the next step.
Copy the file (since it does not exist):
ibook$ cp startupsav.m startup.m
Add a line to the startup file, by typing this (the first line is blank):
ibook$ cat >> startup.m

cd /Users/mweeks/matlab_work
<CTRL-D>
Do not actually type that last line; instead hold down the "ctrl" key and press the "d" key at the same time.
Now we can see the file:
ibook$ cat startup.m
%STARTUPSAV Startup file
% Change the name of this file to STARTUP.M. The file
% is executed when MATLAB starts up, if it exists
% anywhere on the path. In this example, the
% MAT-file generated during quitting using FINISHSAV
% is loaded into MATLAB during startup.
%
% Copyright 1984-2000 The MathWorks, Inc.
% $Revision: 1.4 $ $Date: 2000/06/01 16:19:26 $
%
% load matlab.mat
cd /Users/mweeks/matlab_work
That's it. The next time you start MATLAB, the directory should be set to /Users/mweeks/matlab_work.

If you get an error...

You may get the following error.
Warning: Executing startup failed in matlabrc.
This indicates a potentially serious problem in your MATLAB setup, which should be resolved as soon as possible. Error detected was: MATLAB:load:couldNotReadFile
Error using ==> load
Unable to read file matlab.mat: No such file or directory.
> In matlabrc at 255
>>
If so, don't panic! The problem is that the "matlabrc" program calls the "startup.m" file. The default (which we created above) contians the line load matlab.mat. But this file is not found (or you would not get this error). There are two ways of fixing it: either tell MATLAB to ignore that line, or create the file.
You can comment out line in the "startup.m" file:
% load matlab.mat

Or, if you prefer, you can create the file it looks for (in MATLAB):
>> cd /Applications/MATLAB7/
>> save matlab.mat
