# sosheal
AppleScript to drive BlueStacks to automate batch-healing

You can open either the compiled .scpt file, or the .text version in Script 
Editor.

# Use

Run it directly from Script Editor.

It will run forever until you cancel it. To do that, you have to switch back
from BlueStacks to Script Editor and hit Cmd-. to cancel. There's a small
chance it will jump back to BlueStacks before you can cancel it, but just
switch back again. You always have at least 1 second, and usually 3 seconds.

# Prerequisites

You need BlueStacks, of course.

You also need MouseTools. Easiest way to get it is directly from 
[hamsoft](http://www.hamsoftengineering.com/codeSharing/MouseTools/MouseTools.html),
then `install -m755 ~/Downloads/MouseTools /usr/local/bin/`. Then you have
to run the binary once from Finder to get around the stupid GateKeeper
thing before it'll let you run from the command line or AppleScript.

Next comes giving Accessibility permissions to everything in the universe:

 * Run `MouseTools -location` from Terminal/iTerm/whatever you use
 * Run `do shell script "/usr/local/bin/MouseTools -location"` from Script Editor
 * Run the actual script.

… and each time it asks you to give permission to something, let it take you to
System Preferences Security & Privacy, unlock the Privacy tab, and enable the
checkbox it asked you for.

# Configuration

You'll need to edit most the numbers in the script.

## Coords

Start up BlueStacks, get the game running and set up the way you want. All coords
are global, so you have to have the window in exactly the right place, displaying
the Hospital and Assembly Point, and ideally you should have troops to heal so
that button shows up. Now, for each of the coords, you need to point the cursor
at the icon/button/whatever while running `MouseTools -location` from the shell.
(This means that running BlueStacks full-screen probably won't work, because you
need to be able to see it in the background while typing in the terminal.)

## Delays

The various 1-second and 3-second delays are based on how quickly the game will 
reliably respond to these clicks on my laptop. On your computer, you may need 
more time, or less.

The `repeat 120 times` is how long you want a batch to run, multiplied by the
`delay 3` (and a bit more), so that's a bit over 6 minutes. If a batch takes
longer than 6 minutes, you may recover on the next cycle, may not recover at
all, you may end up ordering every bundle for the game on your credit card and
then deleting your account… If it doesn't recover, you'll need to scroll the
screen or even move some buildings so the failed clicks don't trigger anything
unwanted.

## Troops

The `783` is the number of troops to heal at a time. Check your Assembly 
Point's Timer Help Duration. Multiply that by the number of helps you think 
you can reliably get in 6 minutes (or however long you chose), add a few 
seconds, and go to the Hospital to see how many troops you can heal in that
time (of your highest tier).

# Why this way?

BlueStacks is weird. It looks like instead of accepting raw mouse and key 
info, it accepts high-level Cocoa events, but then installs a tap to capture 
some events and remap them behind its own back. So if you use System Events
to send it a click, or a down arrow, or whatever, it will miss the tap and 
not do what it's supposed to. Typing into text boxes does work reliably, 
at least. But for the clicks, I had to use an external tool. Which means 
global coords rather than window coords, which is why everything is a pain.

There is a way to do ally helps without needing to click the Assembly Point
icon, just by going to the Alliance button, which never moves. But there's no
similar trick to get to the Heal dialog without the Hospital that I know of.
