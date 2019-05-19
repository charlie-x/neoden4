# neoden4
place for neoden4 scripts

dp-neoden4.ulp - rewrite of dangerous prototypes tm220 ulp (modded by jamz and Xinort) to work for the Neoden 4 pick and place.

you have to move your board in eagle to a place where the fiducials in eagle coordinates are mappable in the machines physical space, so no -X,  or -Y all positive, and enough away from 0,0 where the head can physically move. Otherwise it'll cause an error when you try to use the following instructions

I've added a board offset XY to the CSV output page, so that during export you can move the boards offset instead of moving it in eagle, but both work. 

This effectively translates the 0,0 origin of eagle to a new origin that matches the origin of the PCB on the PNP, if your PCB starts at 0,0 in eagle then wherever the lower left corner of the PCB is in PNP XY phyiscal coordinates, this should be set as the offset XY. Remembering that if you are using the camera for visual inspection or fiducials both the nozzle and camera position need to be able to moved too physically by the PNP

the PNP software won't work well with negative coordinates, so make sure your outputted CSV is all positive XY

run ulp from board, load or edit the stack, assign and output.

notes on how to use it after outputting the csv

import the generated csv to the machine.

press edit on the neoden4 software for the new file you just imnported.

make sure the left bottom, and panel1 are set to the XY of component 1 in the Component List(Script will do this)

then add the fidicuals in the fiducials settings as they are in eagle,not as they are in the machine(Script will do this)

click the first component in the list, then click align, it will go to the wrong place thats ok (if it throws an out of bounds error, see note above about where to move the board in eagle), now hit cancel

press "Change to current position" and the PNP will go to the eagle coords of the fidcuials and complain that it cannot find the fiducials, and do you want to do it, say yes.

then  pick the actual location of the fiducial where it physically is on the machine and click save

the pnp  will tries to find the second mark,it should fail again, say yes to find it. search for your second fiducial mark, then click save. (again if you get an out of bounds error, read the note above about where to place the eagle file)

the pnp software should now update all your components to the correct place

then update the left bottom position XY to the newly updated first component XY position and recreate the panel by clicking the panel button. save it, test by single step, or using align.

check everything :)

video version of the text is here https://youtu.be/oF4B8zPS2p8
[![DP-NEODEN4 walkthru](https://img.youtube.com/vi/oF4B8zPS2p8/0.jpg)](https://www.youtube.com/watch?v=oF4B8zPS2p8)

@notes
 generates a loadable csv, tested on machine
 
 user stacks work correctly
 
 special feeders are on again, you may need to use V4.1.3 B9 if stack doesn't load correctly
 
 nearly all parameters are configurable now
 
 From @HolliM
 - A fix so that fiducials are found even if the name or value starts with the letters "FID". I did however not distinguish between top/bottom side. The output always contains both types, but they are easy to delete in Neoden4 software.
 
 - Code to determine the width of the board plus calculating bottom coordinates, so that flipping the board on the Y axis to do bottom side will still have correct (and mirrored) coordinates.
 
 
 
@todo
 bug test
 fix % in reel names breaking the script ( possibly other places too ) 
