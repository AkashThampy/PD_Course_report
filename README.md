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





 

