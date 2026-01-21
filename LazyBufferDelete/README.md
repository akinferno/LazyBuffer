# **LazyBufferDelete - Configuration module**

**Disclaimer**: _Use at your own risk. I am not responsible for any damage caused by use of these macros. That said, I have been working on them for weeks and am using them myself. I would like feedback and suggestions to keep improving them. I can be reached here, or on the Sovol discord server under @AKinferno._

This removes the Buffer MCU and creates dummy modules for all the calls to the buffer module from within Macro.cfg.

# Installation

1) Download _LazyBufferDelete.cfg_ and add upload it to your printer config folder.

2) Edit you _printer.cfg_ file.
    a) Comment out the Sovol _Buffer_Stepper.cfg_ fil by adding a _#_ to the beginning of include statement.
    b) Include the replacement _LazyBufferDelete.cfg_ by typing _[include LazyBufferDelete.cfg]_ in the _printer.cfg_ file.

        #[include Buffer_Stepper.cfg]
        [include LazyBufferDelete.cfg]

3) Save and restart Klipper.  You are done.



# Uninstall 
Decided you prefer loud noises and sporadic operation? All you have to do is change your printer.cfg back to include the original file. You can either comment mine out, or delete the line completely. Save and reboot and you are back to how you bought it. The LazyBuffer.cfg can be deleted if you don't plan to use it again. 

        [include Buffer_Stepper.cfg]
        #[include LazyBufferDelete.cfg]

# CONCERNS
- I have not tested this. There may be some calls in the Macro.cfg that pop up a warning or filament related menu that will only clear if a variable is changed. If you let me know when that is triggered, I can look for that variable and have the dummy call switch it, or make a macro that can switch it to reset the display. It is also possible the variables never switch and everything is fine.


# Credits and Acknowledgements
- Voron Design for making the best printers and having the best 3d printer community 
- Klipper by Kevin O'Connor
- Sovol for making their printers open-source
