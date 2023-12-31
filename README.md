

# Friday 15th Sept

- `sudo shutdown -h` now(shut down immediately)


### Coding Style
+ C has no rules
+ IOCCC 

### Naming Things
- count_active_users()
- `Camel casing` CountActiveUsers()
- u32iIndex - Hungarian notation

### Linux Kernel 
- only one exit to a function

```c
    int func() {
        if(err) goto label; 
    }

```
### How Executables are Made

+ Compiler 
        takes the source (c)   ------> takes it to assembly language
        `gcc -s`
+ Assembler
        assembly(.s) ------> object files
        `gas`,`as`
+ Linker
        objectfiles(.o, .obj) ------> executable
        `ld`, `gold`

### Apllication Binary Interface
- ABI
    - rules your code needs to follow to talk to other code + libraries
    - A software agreement

#### ARM32 C/userspace
r = Registers

r0-r3   ~    First 4 arguments in function(`caller` saved)
r0-r1    ~   return values
r4-r11    ~  general purpose (`callee` saved)
r12-r15   ~~ (stack, LR, PC)

### Kernel ABI
+ OABI - old/original [arm]
+ EABI - new/embedded [armel]
+ hard floating point [armhf]   <== used by `Raspbery Pi OS`

##### armhf
+ system call
+ system call number r7
+ argument           r0-r6
+ return value        r0

## Homework 3

#### Code Density
+ How small can your code can be
+ Still important in embedded systems

#### Instruction Set Architecture
- ARM32     [ 4 byte || 32-bit ]
- THUMB     [ 16-bit ]
- THUMB-2   4 or 2 bytes
- AARCH64 (arm64) 
        mostly different




# Monday 18th Sept


<!-- ![] -->

```s
# equ SYSCALL_EXIT,       1


        .global_start
_start:

        #======================
        # Exit
        #======================
exit:  

@ didnt write everything


```

### Codes run on Terminal for hello_world file

Getting the size `ls la <file>`

`make clean`

`readelf -a <code> | less`

`objdump --disassemble-all <file>`

`strace <file>`



- Write System Call Arguments: 
               - fd
               - pointer to data 
               - length


```s
.equ SYSCALL_EXIT,       1
.equ SYSCALL_WRITE,      4

.equ STDOUT,             1


        .global_start
_start:
        mov     r0, #STDOUT          /* stdout */
        ldr     r1, =hello
        mov     r2, #13              @ length
        mov     r7, #SYSCALL_WRITE
        swi     0x0


        #===============
        #Exit
        #==============
exit:
        mov     r0, #5
        mov     r7, #SYSCALL_EXIT       @ put exit syscall number in register 7 (r7)
        swi     0x0                     @ put exit code

```


`man write`


### Debbuger

`gdb <code>`    ---- gdb debugger

`(gdb) info regis `

`disassem`

`stepi`  ---- go one step


## Back to C CODES

#### Printing an Integer

```c

int x;
printf("%d", x);

//  x == 1234 

// You have to divide by 10 and store it to the [||||]

```s

Dividing: --

10|1234__R4
10|123__R4
10|12__R3
10|1__R2
  |0

Store [1|2|3|4|o]

```

// Read on ASCII Characters + UNICODE


```


# Wednesday 27th Sept.  

```s
+ exit in assembly 

ABI - r0 is first argument
        exit (42),


0x 10400
gcc 10
gcc 12

0x500
ARM32 ~~ 112 bytes
Thumb2 ~~ 88 bytes

```

# Monday 2nd October

#### Reserved Addreses

 7 bit address 0 ..... 127

 0

        reserved
 8

 78
        78..7C --10-bit addreses 


 7F 

#### Firmware

+ Code burned / flashed into device
+ Often controls low-level operation of devices

#### Boot Firware

+ Starts from scratch
+ Gets your system running
+ Kernel people
        + antagonistic relationship with firmware
  

#### Desktop x86
BIOS
UEFI --- Unified Extensible Firware Interface

#### Bootloader 

+ Code just smart enough to load the OS
+ Not many resources stay simple

#### Pi Booting
+ Unusual --- GPU (graphics card) --handles boot
+ Small amount of firmware'
+ Arm chip brought up, but asleep
+ GPU (videocord) loads bootloader


#### Boot
+ Videocore reads bootcode bin
    + /boot partition(FAT)
    + Own OS, RTOS [Real time OS]
    + ThreadX

- ThreadX gets rest of system going
- SDRAM, loads 


#### PI4
Reads initila file from SPI eeprom
Fancier Boot methods
        nexboot
        usb stick
        PCIe bus


#### ARM System Booting
+ UBoot
+ ARM chip boots everything
+ MCO - First stage
+ uBoot - second stge


#### Partitions
+ logically split a storage device into multiple sections
[boot | swap | // | /home]
/dev/sda1   C1
/dev/sda2   D1
            E1

#### FAT
File Allocation Table
        simple filesystem
        1980s
        64l RAM
portable
Filesystem often used
        for/boot
        simple


### IMPORTANT COMMANDS

`ls -la ./<filename>` - Checks the size of the file

![](./Images/lslacmnd.png)


`hexdump -C <./<filename>` -See the raw binary (well, hex) values

![](./Images/hexdumpc.png)

`readelf -a <./filename>` - Look at the elf executable layout

![](./Images/readelf.png)

`objdump ---disassemble-all <./filename>` - See the machine code we generated
-Error encountered while running make on HW3
![](./Images/HW3errorMake.png)

`strace <./filename>` - Trace the system call as the happen
![](./Images/strace.png)

###### HW3 Images

+ Dissasembly of `integer_print`
![](./Images/disassembleintprint.png)

+ Dissasembly of `integer_print_thumb2`
![](./Images/disassembleintprintthumb.png)


### IMPORTANT COMMANDS

# Wednesday 4th October

### HW 4

- error checking
        - print error message
        - exit
  
  `usleep()` --- can enter a low power state
  `uname` 
  linux pi 4,14.1 #122 smp Tue Jan armvl  WU/linux
                                   arch64 


### Disk Spacde
df -h == human readable     512 byte chunks


### Real Time Constraints
What are real time constraints? :-
        -> Time Deadlines the system needs to respond in
        -> Goal not perfomance, but guaranteed response time
        -> Deadlines often are short
                Milliseconds to microseconds
        

### Types of Real Time

##### Hard 
        - miss deadlines, total failure of system
        - people could die if miss deadlines

##### Firm
        - result no longer useful after deadline but occasional misses might be ok 
        - video decoding

##### Soft
        - results you get are gradually less useful after deadline passes


### Who uses Real Time

+ medical devices
+ vehicles
    + airplanes, rockets, cars
+ testing equipment/measurements
+ industrial / SCADA
    + power plants
+ music
+ high speed trading 



# Wednesday 11th October


### Midterm
+ In class
+ Closed book/notes
+ One page of notes
  

  # Midterm Quiz Tips
- Know x-ticks of embedded systems
        + inside
        + fixed purpose
        + resource constrained
        + real-time
        + lots of I/O
  
- Operating System
        + Benefits
             ~  "layers of abstraction"
        +  Downside
             ~  overhead

- C Code
        + look at code know what it is doing
  
        ```c
        open()  errors -1
        read()
        write()
        ioctl()
        close()

        ```
- Code Density
        + why dense code is good in embedded?
        + ARM compressed / embedded
                ISA          THUMB2

- GPIO / i2c
        + know limitations
        + no need to know protocol
  

### Project
- look at pdf on website
        common projects
                + weather station
                + video game
                + 

### HW5

#### Datasheet

- Using the datasheet given

[   |   ]
15      8       

```c

#define HT16K33_OSCILLATOR_ON   ((0x2<<4) | (0x1));

0x21; //enable oscilator
(0x2<<4) | (0x1);

```


```bash
# <!-- terminal code -->

cat notesonterminal.md | wc -l
cat notesonterminal.md | sort | uniq | wc -l
git diff

```

### Getting Real-Time on a Modern System

+ Small system
        - don't run an OS
        - turn off interrupts
        - turn off advanced CPU features
        - lock memory into place
        - 
+ High end Systems
        - Deterministic helper CPUs
        - PRU -beaglebores
        - PIO -pi5
        - embedded system in your `embeded system`
        - 

### Real Time OS

- Special OS good at real time
    - low latency OS calls + interupts
    - Fast/ advanced context switch
    - Job priority system
          - specify som processes have high importance

### Software Worst Case

+ IRQ overhed
    - linux top/bottom half responds to interrupts as fast as possible

### Context Switching

- Switches quickly between processes
- Multitasking
- 100 Hz - 1000 Hz
        Linux (250 Hz) //task switching

### Scheduler

- Picks what program runs next
- Complex :-
  - O(N)
  - O(1)
  - O(log N) -used now "completely fair scheduler"



# Monday 16th October

# Friday 20th October

### SPI (SErial Pheripheral Interface Bus)

- Synchronous Full-Duplex Serial Bus

`Serial` - One bit at a time
`Synchronous` - Clock wire
`Full-duplex` - Send + receive at same time
                        + LCD Display
                        + SD cards
                        + LED Strips
                        + JTAC debugging
                        + A/D converter

- Hardware

controller multiple devices
        <!-- Image here -->

        ![](./Images/Fri20thClass/hardware.png)

        - 4 wire bus (2 power, 2 ground)
        - `SCCK` Serial Clock
        - `MOSI` Master OUT Slave IN
        - `MISO` Master IN Slave OUT
        - `CSO` Chip Select


- Protocol

        = Controller pulls chip select of desired device low     
        = Controller starts clock usually few MHz      
        = Must send + receive at same time
        = Controller sends bits when done deselect + turn off clock
        =
        
- Clock Polarity + Phase

                <!-- Image of how it works here -->

                ![](./Images/Fri20thClass/controllerMultipleDevices.png)

                * Most common 
                        polarity = 0
                        phase = 0

                * Interupts are posible Pi doesn't support it

- Advantages

        - Fast
        - Simple to implement
        - No unique id
        - Unidirectional signals
        - Low power
        - Clock provided by controller

- Disadvantages
        - More pins
        - Short distances
        - No flow control
        - No error reporting
        - No standard

### Doing Homework Setup on PI

        ```c
Pin 23 - `SCCK`
Pin 19 -`MOSI `
Pin 21 -`MISO `
Pin 24 - `CEO`
Pin 26 - `CEI`
                        ```c
                        /dev/spider(bus(pin), slave(pin))

                        int mode = SPI_MODE_0;
                        'ioctl(fd, SPI_IOC_WRMODE; $mode) 




                        
                        ```

        ```
                <!-- Image of seting upp the SPI -->

                ![](./Images/Fri20thClass/settingSPI.png)

- MCP 3008
        8 port 10-bit SPI
                A/D converter
        
        Send 3 bytes 
                
        ```c
        Start bit_________________________________
        |xxxxx| |xxxx|  |xxxx| |x098| |7654| |3201|
        |_________________________________________|

        ___________________________________________
        |xxxxx| |xxxx|  |xxxx| |x098| |7654| |3201|
        |_________________________________________|
        <!-- geting `7654 3201` to position 1 -->
        
        int x = (buffer(1) << 8);
        deg_Farenheight = ((deg_C)* 9/5 ) + 32;
        printf("%lf"/n)
        ```

        ![](./Images/Fri20thClass/codestogothru.png)


# Friday 20th October


# Monday 23rd October

```c
 memset()
        <!-- initializing to zero -->

```



# Monday 23rd October




# Friday 3rd November

Midterm on 17th 


### HW9
         measuring temp
         result on a display

         either 1-wire / SPI
         modular code
         multiple C files
         self contained code
         working groups
         git


         ```c
        // # include "temp.h" file

         double get_temperature(void) {
                // code here
         }
         
        //  set the display and geting temp seperately

         ```


        ```c
        double t;

        t = 65.2;

        // Using division to display the results
        hundreds = t/100;
        remainder = t % 100;
        tens = remainder / 10;


        sprintf(str, "%lf, t)
        
        
        ```

### Computer Security

##### Types of Security Issues

        + Crash
        + Demand of Service (DOS)
        + User account compromise
        + Root account compromise  (rootkit)
        + Remote root compromise

##### Information Leakage/ Side Channel Attacks

        + Side channel
                Leak info
                Radio waves
                Power supply
        + Timing
                Different paths through
                Code takes different time

        + Meltdown/ Spectre
                modern 0o0 processors
                speculative execution

        + Deceptive code
                Typo squatting
                Javascript / npm
                Sneak code into Linux kernel
                Uot Minnesotta tried to sneak code in
                Source code unicode

        + Finding Bugs
                Crashes
                Source code inspection
                Watching lists/ bug reports
                Static checker
                Dynamic checkers/ valgrind
                Festing
                Fuzzing
                <!-- Social Engineering -->
                       
                
# Friday 3rd November



# Monday 6th November


##### Social Engineering
        + Talking your way in
        + Phishing attacks
        + "The art of deception"

##### Case Studies
        - 2010 IEEE SoSP
                Fuzzed a car (CANBUS) - control brakes
                                      - heating / cooling
                                      - windows / locks
                                      - anti lock
                                      - cruise control
                                      - pre-crash detectiomn
                                      - instrument panel
                                      - stop engine

        - Stuxnet
                SCADA (Supervisory Control + Data Acquisition) 
                                        - Via internet plus USB key
                                        - Four zero-day vulnebility
                                        - Was to activate on a specific Siemens SCADA


        Software Bugs
                + Not all issues are security
                + Bad code/ accidental
                + User interface

        Automotive
                + Bugs in Toyota firmware
                        (engine accelerated without stopping)
                        VIN
                + Airplanes
                        AA Flight 965
                                (waypoint R) --computer picked the wrong waypoint and crashed
                        AirFrance 447
                + Military
                        Patriot Missile
                        Yorktown Smart Ship (1997) ---Windows NT
                        

# Monday 6th November



# Wednesday 8th November

Where security is affected:-
                + Financial
                
                + Power
                   2003 Blackout
                   race condition in server

        
Code Safety Standards
        + Aironics
        + Industrial
        + Railway 
        + Nuclear
        + Medical
        + Automotive

                1. Aviation
                        D0-178B/DO-17bC

                        Catastrophic
                        Hazardous
                        Major
                        Minor(inconvinience)

                2. Automotive
                        ISO 26262

                        - definitions
                        - management
                        - safety life cycle
                        - processes
                        - Severity
                                S0 -No injuries
                                S3 -Not survivable
                        - Exposure
                                E0 - Not likely
                                E4 - Highly likely

                3. Medical
                        IEC 62304

                        - Avoid using software of unknown pedigree

                        Class A

                        Class C

### Writing Good Code
 - Various Books
 - Comment your code
 - Formatting
 - Exact variable types
        int32_t            int
- Avoid undefined behavior
- Tools to enforce

### MISRA -  C
 Motor Industry Software Reliability Asssociation

 Guidlines:-
        Mandatory
        Required
        Advisory

        use int32_t
        avoid functions that can fail -- `malloc()`
        maintainable coding styles

Compliance:- 
        All mandatory must follow
        Required rules you can break formal writeup

        MISRA 2012
                143 rules
                16 directives

Documentation:
        Comment code
        Auto-generate docs from code commments

Good code example
        `Space Shuttle`
                - computers were good
                - lots of testing
                - only 3 bugs,,,.....,,,


# Wednesday 8th November



# Monday 20th November

##### Energy and Power

Power = Energy / Time

Power is `instantaneous`

###### Units
 Energy - Joules, kWh(3.6MJ)
        - Therm(105.5MJ)
        - 1 Ton TNY(4.26J)
        - eV(1.6x10^-19 J)
        - BTU(1055J)
Power   - Watts (J/s)
        - Horsepower(746W)
        - Tons of refrigiration(12,000BTU/h)

Baterries for Embedded Systems
* LiOn Lithium Ion
* Li-Po Lithium Polymer (rechargable)
* controller to handle recharging

###### CMOS

MOSFET
<!-- Diagram here -->

CMOS Power
Dynamic Power
P = C/_\ V Vdd & F
        & - activity factor

![](Images/powermosfets.jpg) 

###### Static Power
Leakage current

Pstatic = IleakageVdd

###### Thermal
temperature closely related to power

___________|Idle___|Load___|Time___|Energy__
RPI        |3.0W   |3.3W   |23.5s  |77.6J
Overdo     |2.6    |
Beagleboard|       |
Pandaboard |       |
Chromebook |       |

![](Images/powerdiffboards.jpg)

Least Energy ---Pandaboard
Fastest  ---- Chromebook

###### Measure
measure voltage + current
P = IV





# Monday 20th November










