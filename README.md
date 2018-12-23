# neoden4
place for neoden4 scripts

dp-neoden4.ulp - rewrite of dangerous prototypes tm220 ulp (modded by jamz) to work for the Neoden 4 pick and place.

you have to move your board in eagle to a place where the fiducials in eagle coordinates are mappable in the machines physical space, so no -X,  or -Y all positive, and enough away from 0,0 wherre the head can physically move. Otherwise it'll cause an error when you try to use the following instructions

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

check everything :)



@notes
 generates a loadable csv
 
@todo
   verify
   
