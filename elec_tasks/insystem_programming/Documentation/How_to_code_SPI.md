#  How to code SPI

Serial Peripheral Interfacing is one of the most used serial communication protocols, and very simple to use!

In SPI, every device connected is either a Master or a Slave.
The Master device is the one which initiates the connection and controls it. Once the connection is initiated, then the Master and one or more Slave(s) can transmit and/or receive data. As mentioned earlier, this is a full-duplex connection, which means that Master can send data to Slave(s) and the Slave(s) can also send the data to the Master at the same time.
</br>
SPI uses 4 pins for data communication. So let’s move on to the pin description. 

### Pin Description
The SPI typically uses 4 pins for communication, wiz. MISO, MOSI, SCK, and SS. These pins are directly related to the SPI bus interface.

#### MISO – MISO stands for Master In Slave Out. MISO is the input pin for Master AVR, and output pin for Slave AVR device. Data transfer from Slave to Master takes place through this channel.

#### MOSI – MOSI stands for Master Out Slave In. This pin is the output pin for Master and input pin for Slave. Data transfer from Master to Slave takes place through this channel.

#### SCK – This is the SPI clock line (since SPI is a synchronous communication).

#### SS – This stands for Slave Select.

Now we move on to the SPI of AVR!

### The SPI of the AVR
The SPI of AVRs is one of the most simplest peripherals to program. As the AVR has an 8-bit architecture, so the SPI of AVR is also 8-bit. 

![spi_atmega8.png](https://github.com/sabSAThai/Advitiya/blob/master/images/spi_atmega8.png)


### Register Descriptions
The AVR contains the following three registers that deal with SPI:

#### SPCR – SPI Control Register – This register is basically the master register i.e. it contains the bits to initialize SPI and control it.

#### SPSR – SPI Status Register – This is the status register. This register is used to read the status of the bus lines.

#### SPDR – SPI Data Register – The SPI Data Register is the read/write register where the actual data transfer takes place.



### The SPI Control Register (SPCR)

![SPCR.png](https://github.com/sabSAThai/Advitiya/blob/master/images/SPCR.png)

###### Bit 7: SPIE – SPI Interrupt Enable
The SPI Interrupt Enable bit is used to enable interrupts in the SPI. Note that global interrupts must be enabled to use the interrupt functions. Set this bit to ‘1’ to enable interrupts.

###### Bit 6: SPE – SPI Enable
The SPI Enable bit is used to enable SPI as a whole. When this bit is set to 1, the SPI is enabled or else it is disabled. When SPI is enabled, the normal I/O functions of the pins are overridden.

###### Bit 5: DORD – Data Order
DORD stands for Data ORDer. Set this bit to 1 if you want to transmit LSB first, else set it to 0, in which case it sends out MSB first.

###### Bit 4: MSTR – Master/Slave Select
This bit is used to configure the device as Master or as Slave. When this bit is set to 1, the SPI is in Master mode (i.e. clock will be generated by the particular device), else when it is set to 0, the device is in SPI Slave mode.

###### Bit 3: CPOL – Clock Polarity
This bit selects the clock polarity when the bus is idle. Set this bit to 1 to ensure that SCK is HIGH when the bus is idle, otherwise set it to 0 so that SCK is LOW in case of idle bus.

##### Bit 2: CPHA – Clock Phase
This bit determines when the data needs to be sampled. Set this bit to 1 to sample data at the leading (first) edge of SCK, otherwise set it to 0 to sample data at the trailing (second) edge of SCK.

##### Bit 1,0: SPR1, SPR0 – SPI Clock Rate Select
These bits, along with the SPI2X bit in the SPSR register (discussed next), are used to choose the oscillator frequency divider, wherein the fOSC stands for internal clock, or the frequency of the crystal in case of an external oscillator.


### The SPI Status Register (SPSR)

![SPSR.png](https://github.com/sabSAThai/Advitiya/blob/master/images/SPSR.png)

The SPI Status Register is the register from where we can get the status of the SPI bus and interrupt flag is also set in this register. Following are the bits in the SPSR register.

###### Bit 7: SPIF – SPI Interrupt Flag
The SPI Interrupt Flag is set whenever a serial transfer is complete. An interrupt is also generated if SPIE bit (bit 7 in SPCR) is enabled and global interrupts are enabled. This flag is cleared when the corresponding ISR is executed.

###### Bit 6: WCOL – Write Collision Flag
The Write COLlision flag is set when data is written on the SPI Data Register (SPDR, discussed next) when there is an impending transfer or the data lines are busy.
This flag can be cleared by first reading the SPI Data Register when the WCOL is set. Usually if we give the commands of data transfer properly, this error does not occur. We will discuss about how this error can be avoided, in the later stages of the post.

###### Bit 5:1
These are reserved bits.

###### Bit 0: SPI2x – SPI Double Speed Mode
The SPI double speed mode bit reduces the frequency divider from 4x to 2x, hence doubling the speed. Usually this bit is not needed, unless we need very specific transfer speeds, or very high transfer speeds. Set this bit to 1 to enable SPI Double Speed Mode. This bit is used in conjunction with the SPR1:0 bits of SPCR Register.


###### The SPI Data Register (SPDR)

![SPDR.png](https://github.com/sabSAThai/Advitiya/blob/master/images/SPDR.png)

The SPI Data register is an 8-bit read/write register. This is the register from where we read the incoming data, and write the data to which we want to transmit.
</br>
The 7th bit is obviously, the Most Significant Bit (MSB), while the 0th bit is the Least Significant Bit (LSB).

Data Modes
The SPI offers 4 data modes for data communication, wiz SPI Mode 0,1,2 and 3, the only difference in these modes being the clock edge at which data is sampled. This is based upon the selection of CPOL and CPHA bits.

![datamodes.png](https://github.com/sabSAThai/Advitiya/blob/master/images/datamodes.png)


### The Slave Select (SS’) Pin

SS’ (means SS complemented) works in active low configuration. Which means to select a particular slave, a LOW signal must be passed to it.
</br>
**When set as input, the SS’ pin should be given as HIGH (Vcc) on as Master device, and a LOW (Grounded) on a Slave device.**
When as an output pin on the Master microcontroller, the SS’ pin can be used as a GPIO pin.

### SPI Coded!
We only discussed about the register description, hardware connections etc. of the SPI. Now lets see how we Code it!

#### Enabling SPI on Master

``` c

// Initialize SPI Master Device (with SPI interrupt)
void spi_init_master (void)
{
    // Set MOSI, SCK as Output
    DDRB=(1<<5)|(1<<3);
 
    // Enable SPI, Set as Master
    // Prescaler: Fosc/16, Enable Interrupts
    //The MOSI, SCK pins are as per ATMega8
    SPCR=(1<<SPE)|(1<<MSTR)|(1<<SPR0)|(1<<SPIE);
 
    // Enable Global Interrupts
    sei();
}

```

``` c

Enabling SPI on Slave
// Initialize SPI Slave Device
void spi_init_slave (void)
{
    DDRB = (1<<6);     //MISO as OUTPUT
    SPCR = (1<<SPE);   //Enable SPI
}

```

``` c
Sending and Receiving Data
//Function to send and receive data for both master and slave
unsigned char spi_tranceiver (unsigned char data)
{
    // Load data into the buffer
    SPDR = data;
 
    //Wait until transmission complete
    while(!(SPSR & (1<<SPIF) ));
 
    // Return received data
    return(SPDR);
}

```
