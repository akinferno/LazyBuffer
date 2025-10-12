# **LazyBuffer**
An override of the Sovol Buffer/Feeder to make it behave more like I expected it to behave when it was shown. With several enhancements I dreamed up at the time.

This was created to with the intent of providing full backward compatibility with all system calls from the original Sovol Buffer_Stepper.cfg file. Since I am not using the original pin definitions, the two options were to use unused pins on the board, which restrict their use for future mods, or use virtual pins. I opted for virtual pins. So, here is what you need to do to get started:

# INSTALLATION
1) Install Virtual Pins by Pedrolamas. https://github.com/pedrolamas/klipper-virtual-pins
    a) Using a terminal program (putty, mobaterm, etc.), access the Sovol SV08 Max.
       - Username: sovol
       - Password: sovol
    b) Clone the Virtual Pin repository with the following commands:
   
        cd ~
        git clone https://github.com/pedrolamas/klipper-virtual-pins.git
        ./klipper-virtual-pins/install.sh

2) Download _LazyBuffer.cfg_ and add upload it to your printer config folder.
   
4) Open the _LazyBuffer.cfg_ and insert you buffers **canbus_uuid:** under _**[mcu buffer_mcu]**_

5) Edit you _printer.cfg_ file.
    a) Comment out the Sovol _Buffer_Stepper.cfg_ fil by adding a _#_ to the beginning of include statement.
    b) Include the replacement _LazyBuffer.cfg_ by typing _[include LazyBuffer.cfg]_ in the _printer.cfg_ file.

        #[include Buffer_Stepper.cfg]
        [include LazyBuffer.cfg]

6) Save and restart Klipper.  You are done.


# EXPANDED FEATURES
- Buffer now syncs with the extruder motor to QUIETLY and smoothly load filament throughout a print.
- Reduce current to buffer stepper to prevent heat buildup.
- Automatic Loading and Unloading
- Clog Detection and Filament Runout detection
- Enable/Disable any of these functions, including the buffer itself, it you want to bypass it
- Enhanced functionalty of the button on the buffer


# BUFFER BUTTON MODES EXPLAINED
**Basic Button Fuction** _- If Advance_Button_Enabled pin is false or turned off._
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
    - Green - AUTO_LOAD_FILAMENT
    - Blue - RESUME
    - Red - AUTO_UNLOAD_FILAMENT
      
  **Idle**
    - Green - AUTO_LOAD_FILAMENT
    - Red - AUTO_UNLOAD_FILAMENT

# Credits and Acknowledgements
- Klipper by Kevin O'Connor
- Virtual Pins by Pedrolamas
- Assiting with Testing, troubleshooting and passing ideas:
  - @wildBill
  - @uniqueacid
