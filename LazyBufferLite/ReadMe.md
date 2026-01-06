# **LazyBuffer Lite - Configuration module**

**Disclaimer**: _Use at your own risk. I am not responsible for any damage caused by use of these macros. That said, I have been working on them for weeks and am using them myself. I would like feedback and suggestions to keep improving them. I can be reached here, or on the Sovol discord server under @AKinferno._

Lite version was made with the following changes:
- Removed all button functionality. The buffer now just synces when enabled, and does nothing when disabled (There is a switch in Mainsail where fans are located).
- Removed all automated loading and unloading functions

So what is a brief synopsys of what is left:
- On startup, the buffer is synced with the extruder. The buffer auto correction of rotation distance only occurs when printing. 
- If the extruder advances, the buffer advances. If extruder retracts, buffer retracts.
- Easy disable the buffer with a toggle button in the fan area of Mainsail/Fluidd.  Message me on any of the socials if you want to make it defaulted off.
- !! Remember, automation was removed. If you manually extrude or retract a bunch of filament outside a print, it may not correct for variations in rotation distance!!

# Installation
1) Install Virtual Pins by Pedrolamas. https://github.com/pedrolamas/klipper-virtual-pins
   
    a) Using a terminal program (putty, mobaterm, etc.), access the Sovol SV08 Max.
   
       Username: sovol
       Password: sovol
   
    b) Clone the Virtual Pin repository with the following commands:
   
        cd ~
        git clone https://github.com/pedrolamas/klipper-virtual-pins.git
        ./klipper-virtual-pins/install.sh

3) Download _LazyBufferLite.cfg_ and add upload it to your printer config folder.
   
4) Open the _LazyBufferLite.cfg_ and insert you buffers **canbus_uuid:** under _**[mcu buffer_mcu]**_. _If you don't know your UUID, open the original **Buffer_Stepper.cfg** file and copy it from there._
      
        [mcu buffer_mcu]
        canbus_uuid: <insert your CAN ID here>

5) Edit you _printer.cfg_ file.
    a) Comment out the Sovol _Buffer_Stepper.cfg_ fil by adding a _#_ to the beginning of include statement.
    b) Include the replacement _LazyBufferLite.cfg_ by typing _[include LazyBufferLite.cfg]_ in the _printer.cfg_ file.

        #[include Buffer_Stepper.cfg]
        [include LazyBuffer.cfg]

6) Save and restart Klipper.  You are done.



# Uninstall 
Decided you prefer loud noises and sporadic operation? All you have to do is change your printer.cfg back to include the original file. You can either comment mine out, or delete the line completely. Save and reboot and you are back to how you bought it. The LazyBuffer.cfg can be deleted if you don't plan to use it again. 

        [include Buffer_Stepper.cfg]
        #[include LazyBuffer.cfg]


# Credits and Acknowledgements
- Voron Design for making the best printers and having the best 3d printer community 
- Klipper by Kevin O'Connor
- Virtual Pins by Pedrolamas
- Assiting with Testing, troubleshooting and passing ideas:
  - @uniqueacid
- Sovol for making their printers open-source
