# **LazyBuffer configuration module** _v0.25_
_(version 0.XX indicates Beta status. Once all the kinks are worked out, it will be updated to v1.0)_

An override of the Sovol Buffer/Feeder to make it behave more like I expected it to behave when it was shown. With several enhancements I dreamed up at the time.

This was created to with the intent of providing full backward compatibility with all system calls from the original Sovol Buffer_Stepper.cfg file. Since I am not using the original pin definitions, the two options were to use unused pins on the board, which restrict their use for future mods, or use virtual pins. I opted for virtual pins. So, here is what you need to do to get started:

**Disclaimer**: _Use at your own risk. I am not responsible for any damage caused by use of these macros. That said, I have been working on them for weeks and am using them myself. I would like feedback and suggestions to keep improving them. I can be reached here, or on the Sovol discord server under @AKinferno._


# Installation
1) Install Virtual Pins by Pedrolamas. https://github.com/pedrolamas/klipper-virtual-pins
   
    a) Using a terminal program (putty, mobaterm, etc.), access the Sovol SV08 Max.
   
       Username: sovol
       Password: sovol
   
    b) Clone the Virtual Pin repository with the following commands:
   
        cd ~
        git clone https://github.com/pedrolamas/klipper-virtual-pins.git
        ./klipper-virtual-pins/install.sh

3) Download _LazyBuffer.cfg_ and add upload it to your printer config folder.
   
4) Open the _LazyBuffer.cfg_ and insert you buffers **canbus_uuid:** under _**[mcu buffer_mcu]**_. _If you don't know your UUID, open the original **Buffer_Stepper.cfg** file and copy it from there._
      
        [mcu buffer_mcu]
        canbus_uuid: <insert your CAN ID here>

5) Edit you _printer.cfg_ file.
    a) Comment out the Sovol _Buffer_Stepper.cfg_ fil by adding a _#_ to the beginning of include statement.
    b) Include the replacement _LazyBuffer.cfg_ by typing _[include LazyBuffer.cfg]_ in the _printer.cfg_ file.

        #[include Buffer_Stepper.cfg]
        [include LazyBuffer.cfg]

6) Save and restart Klipper.  You are done.


# Expanded Features
- Buffer now syncs with the extruder motor to QUIETLY and smoothly load filament throughout a print.
- Reduced current to buffer stepper to prevent heat buildup.
- Automatic Loading and Unloading
- Clog Detection and Filament Runout detection
- Enable/Disable any of these functions, including the buffer itself, it you want to bypass it
- Enhanced functionalty of the button on the buffer


# Buffer Button Modes Explained
**Basic Button Fuction** _- If Advance_Button_Enabled pin is false or turned off._ (COMING SOON - Advanced Mode is the only mode at the moment)
- Press the button advance the filament buffer in 5mm increments
- Hold the button to execute what was the Long Unload function.


**Advanced Button Function** _- If Advance_Button_Enabled pin is true or turned on._
- Pressing the button changes modes
- Holding the button executes the mode action.
- Modes are identified as Default, Green, Blue and Red.
- Default is identified as a solid LED color. Green, Blue and Red by blinking LED of the selected color. Mode reverts to Default after 10sec of inactivity or execution of action.
- Mode commands vary based on printer status.

  **Printing** 
    - Blue - PAUSE
    - Red - CANCEL_PRINT
      
  **Paused**
    - Green - Extrude 5mm, repeat until released
    - Blue - RESUME
    - Red - Retract 5mm, repeat until released
      
  **Idle**
    - Green - AUTO_LOAD_FILAMENT
    - Red - AUTO_UNLOAD_FILAMENT


# Uninstall 
Decided you prefer loud noises and sporadic operation? All you have to do is change your printer.cfg back to include the original file. You can either comment mine out, or delete the line completely. Save and reboot and you are back to how you bought it. The LazyBuffer.cfg can be deleted if you don't plan to use it again. 

        [include Buffer_Stepper.cfg]
        #[include LazyBuffer.cfg]


# Credits and Acknowledgements
- Klipper by Kevin O'Connor
- Virtual Pins by Pedrolamas
- Assiting with Testing, troubleshooting and passing ideas:
  - @wildBill
  - @uniqueacid
- Sovol for making their printers open-source


_Why Lazy?_

_William Washington told me "Laziness breeds efficiency." I argued with him about that when I was young. His point was when people think something is cumbersome, inefficent or takes too much effort to accomplish, they create easier more efficient ways of doing so. 
Started with my Lazy Cams for my Voron 0. I made them because I didn't like having to find a tool to open my Voron. I went throuch several iterations before settling on what I think is a must have mod for the Voron 0.
Enter the Sovol SV08 Max buffer.  Not only was it loud and obnoxious, but it wasn't efficient. There were 3 sensors and they weren't being used effectively. So I spent weeks making the LazyBuffer macros, after being told it couldn't be done._

# Bug Chase
**v0.25**
* Attempted to correct the PAUSE issue. It seemed to be built into the Sovol PAUSE macro. I disable the filament sensorused by the Sovol macros to prevent automated actions. Should not affect new LazyBuffer macros.
* 0.24 'fixes' have not  been tested.

**v0.24**
* LED status' aren't tracking properly after execution. M400 is causing this issue as it pauses commands until previous step is complete. Rewrite of LED management planned.
* Possible overlap of command loops which M400 didn't fix. Implemented a different method to prevent, but not yet tested. If it works, M400 may be removed, fixing this and LED issue.
* Basic control option not implemented.
* During print, when pausing with printer LCD instead of the buffer button, the buffer attempted to retract filament. Some conflict with Sovol macros that needs to be addressed.  Pause worked fine from LazyBuffer advanced function.
