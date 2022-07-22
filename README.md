# PD_Course_report








Setup part: 

	Importing required package
package require openlane 0.9
	Doing the required design setup
Prep -design picorv32 




Synthesis: 

Command : run_synthesis 
Successfully completed synthesis part and completed the first assignment to get the flop ratio. 

![image](https://user-images.githubusercontent.com/107097885/175575126-3b24bced-c3ed-4973-99a5-5411a03b33e0.png)

 

Num of flops / total cells = 1613/14876 =  0.10842968539  or 10.84%
Floor plan:  

Completed the floor plan and tried to tweak the variables to see the differences in magic layout. 
set ::env(FP_ASPECT_RATIO) 1  ## default
set ::env(FP_IO_MODE) 1;     ##deafult 

Default magic layout:  (with the above default values) 

 ![image](https://user-images.githubusercontent.com/107097885/175575206-2261dadf-8567-498c-b299-5abec3ef504c.png)
![image](https://user-images.githubusercontent.com/107097885/175575226-1365c331-dd3f-4e55-a86e-51ff6457e3b9.png)

 
With Variables tweaked. 
 
set ::env(FP_ASPECT_RATIO) 0.8
set ::env(FP_IO_MODE) 0;     

Magic layout: 


We can see that the aspect ratio and the pin placement has changed honouring the settings.  
 
 ![image](https://user-images.githubusercontent.com/107097885/175575260-68f9a0b3-fc56-4f68-b6bc-29d4de7308a6.png)






 
Placement: 


Successfully completed placement and made sure that the std cells are all placed in the proper std cell rows. 

Placement done with a placement density of 0.55
 
![image](https://user-images.githubusercontent.com/107097885/175575288-aedb7d54-787a-43c0-becc-da41a73ebd79.png)
![image](https://user-images.githubusercontent.com/107097885/175575311-f374fb62-ebff-47f4-bb46-7b5c2a97f829.png)


 
Tried to tweak the variable   PL_TARGET_DESITY 0.4
Noticed the placement density has got a bit relaxed. 

 
![image](https://user-images.githubusercontent.com/107097885/175575352-26427b8b-508b-4f57-acae-4a63ec59f1dd.png)





 Successfully cloned vsdstdcelldesign from github. 
 
 ![image](https://user-images.githubusercontent.com/107097885/175576938-ea630d3c-1904-49ba-816b-98572c6101bc.png)

With SYNTH_STRATEGY "DELAY 0". Achieved 0 slack...

![image](https://user-images.githubusercontent.com/107097885/178073667-7098cb87-ae1e-4c83-bcb8-68216f8852cd.png)



Opening the inveter layout through magic and undertanding the layers. DRC clean design sa we see DRC= 0 

![image](https://user-images.githubusercontent.com/107097885/175579519-70ba1d8d-32f9-4c9b-998a-ce84c1ae30b1.png)

Editing the layout to understand the DRC violations popping: 

![image](https://user-images.githubusercontent.com/107097885/177954148-314eb76f-7eb3-4be7-8b03-b75ab16e4605.png)


Extracted spice deck from the inverter layout from magic using the below commands: 
extract all
ext2spice cthresh 0 rthresh 0
ext2spice 

made necessary changes and below is the final spice file. Doing a transient simualtion by applying a pulse 3.3v to the input. 


![image](https://user-images.githubusercontent.com/107097885/177978928-e6256424-ba14-4156-9638-1e631d54c0d7.png)



Obtained the required output, where y is plotted againts input a and y seems to be inverting with respect to which is the expected behaviour in inv cells. 

![image](https://user-images.githubusercontent.com/107097885/177978750-71dc580c-ad8c-40bc-abbd-e8b70df8483c.png)


Used the spice generated w/f to compute slew and cell delay.

Slew= time_at_(80%of w/f-20%w/f)
Cell delay_rise = time_at(50%o/p_rise-50%input_fall) and vice versa for fall_cell dealy

Input rise slew: 4.08006e-9-4.02019e-9 =  59.87ps
Input Fall slew: 2.18e-9 - 2.12e-9 = 60ps
Output rise slew: 2.20377e-9-2.16169e-9 = 42ps
Output False slew: 4.06826e-9-4.04042e-9 = 27ps


Cell delay rise = 2.18446e-9-2.1497e-9 = 34.76ps
Cell dealy fall = 4.05432e-9-4.05001e-9 = 4.31ps

Eg w/f capturing cell rise delay:

![image](https://user-images.githubusercontent.com/107097885/177988829-ae0426b1-4a34-43a9-83a3-0c21a6573784.png)


Donwloaded and untarred the drc_test kit and opened MET3.mag in magic , checking DRC violations reasons using console window: 

![image](https://user-images.githubusercontent.com/107097885/178001097-8ebbae25-ddcf-4199-8779-dc3dc4476b75.png)


Conversion of layout to lef steps


1. Making sure that the inv layouts input and output ports are correctly placed on the intersection of tracks of li layer. 

![image](https://user-images.githubusercontent.com/107097885/178006500-104079ae-afe8-4d68-ad3d-9a11614d3a73.png)



2. Width of std cell = odd multiple of x pitch  , height of std cell is odd  multiple of y pitch -> verified. 



3. Definging ports in required layers: 

![image](https://user-images.githubusercontent.com/107097885/178008265-16470849-8fcd-493f-b10d-7d2eaabcd750.png)


4. Defining direction and use of ports: 

![image](https://user-images.githubusercontent.com/107097885/178009719-294498a3-78e1-469f-9885-03d9e30e9872.png)



# Finally writing down the lef: 

![image](https://user-images.githubusercontent.com/107097885/178011087-f4d38b7a-2588-4aef-a6e5-da9fcc96f14e.png)


Lef content:  Defined ports with direction and type is honored in the lef. 

![image](https://user-images.githubusercontent.com/107097885/178011296-2b151487-5238-4216-81c7-a6579421b523.png)

# Plugging in the custom created inv lef and libs: 

# 1. Synthesis part -> doner successfully (mapped using typical lib corner) and lef written out. 


![image](https://user-images.githubusercontent.com/107097885/178058139-2ff7d0ff-f432-45c4-9af4-24d732aa7995.png)

But with the default synthesis settings, it seems like we have high -ve slack vioaltions. See the snip below and highlights:

![image](https://user-images.githubusercontent.com/107097885/178064557-ac3f9314-aea3-47fe-a1de-bf683321ea8c.png)

Trying to change some synthesis settings associated with area of cell, size of cell, buffering of reset paths to see if delay is improved. 

![image](https://user-images.githubusercontent.com/107097885/178065074-451608bc-a410-4298-9af4-42b6914e9369.png)

Finally SYNTH_STRATEGY DELAY 0 helped. 

![image](https://user-images.githubusercontent.com/107097885/178073935-6343c914-2528-45fa-9674-aba1f557e1ed.png)


Floor plan (using new command init_floorplan) steps till placement placement done successfully, custom inv cell highlighted in the gui below in magic
init_floorplan
place_io,
global_placement_or,
detailed_placement,
tap_decap_or,
detailed_placement

![image](https://user-images.githubusercontent.com/107097885/178092485-18acc390-2bd5-4bb2-bdfa-b17983ace378.png)


#running open sta for post synthesis timing analysis: 

![image](https://user-images.githubusercontent.com/107097885/178094722-a6d750fd-9aa6-4e76-85f1-3aedc2a342fc.png)

Optimizing synthesis to reduce setup violations. 
SYNTH_MAX_FANOUT 4 etc. 


# CTS successfully done. 


![image](https://user-images.githubusercontent.com/107097885/178097825-fa0e1974-7283-49f0-8c3f-11175897e9cb.png)




#Created a new netlist for further stages after CTS. 

![image](https://user-images.githubusercontent.com/107097885/178098003-d7b94cd7-b0d9-4da0-ac70-ca4d18092ecc.png)

# few checks post cts to verify the results: 

![image](https://user-images.githubusercontent.com/107097885/178098437-bd4f9514-7700-41e2-b61c-f6e11fef6983.png) 



 Launching openroad, loading lef and cts def and  dumping a db
 
 ![image](https://user-images.githubusercontent.com/107097885/178100778-01d6555f-2b9d-4de4-b171-89655b167885.png)
 
 
 Observing -ve hold slack after cts which needs to be fixed. 
 
 ![image](https://user-images.githubusercontent.com/107097885/178101216-8ec848ab-2087-4891-98a1-b9e8b223b7c1.png)



Reloading the database for PDN and routing:

![image](https://user-images.githubusercontent.com/107097885/180350156-aa3b9a3f-9b66-440b-ab57-f13b6b55c536.png)

Verified that we are at corret stage (after CTS) by checking the def. (pointed to cts def)

Going for gen_pdn for power grid generation:

![image](https://user-images.githubusercontent.com/107097885/180350276-f4193f30-96a5-4538-9341-13f06e57e15f.png)


# Successfully done pdn. 

![image](https://user-images.githubusercontent.com/107097885/180350384-cff3a6ab-ccd1-4e06-979e-5113daf518ac.png)


# Making sure about rail pitch matches with std height

![image](https://user-images.githubusercontent.com/107097885/180350607-7e4ee42f-5796-447d-9a68-d50f90416858.png)
