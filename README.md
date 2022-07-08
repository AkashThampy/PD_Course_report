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



