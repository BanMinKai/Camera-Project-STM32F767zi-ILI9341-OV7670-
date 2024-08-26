# Camera-Project-STM32F767zi-ILI9341-OV7670-
This is an project attempt following a Youtube project (https://www.youtube.com/watch?v=MqtJbraAlOU&amp;t=1s)
The project is about making a digital camera in which we have an STM32 microcontroller as the processor; an ILI9341-driver-based LCD as an display; and an OV7670 camera.

There are a few simplified versions for the camera online but most of them are implemented using STM32F4 microcontrollers.
When I attempted the project using STM32F7, I encountered a few F4-to-F7 porting challenges.

For example, the F7 series is a more advanced microcontroller series, so  
they have Cache memory and a hardware unit called Memory Protection Unit (MPU). This MPU unit messes up with how another hardware unit called FSMC (aka FMC) works. And, the FSMC 
is the unit used to interface with the S7 microcontroller with ILI9341-based LCD if you want to do 16-bit interface approach like I did. I really had to look for the solution of this MPU issue
 in the Chinese-Github-like community called CSDN. 

Another challenge is a hardware issue that makes images are being misplaced over time, leading to shifting and breaking up the images. The issue can be solved by writing to a specific register of the ILI9341 whenever
the OV7670 camera is done sending a frame. In other words, inserting a line of LCD_CmdWrite(ILI9341_MEMORYWRITE_register) inside the HAL_DCMI_FrameEventCallback.

Those two issues are not the only issues I faced howeever, they are remarkable because they are hardware issues and they teach me the value of having full control over the devices you are using.
Both issues can only be solved if there is a deep understanding of how the devices work.

In the end, my final product is able to take frames (images) captured by the camera, and the microntroller then channels these captured frames to the TFT-LCD and then display them.
I have not been able to incorporate buttons, saving image or RTOS for a full-fledged application system. Basically my product is a rudimentary live camera, I can see what the camera sees, but
I cannot take any picture and save them. 
