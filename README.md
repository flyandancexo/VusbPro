# VusbPro - Enhanced V-USB, a software-based USB device
### High Speed USBasp firmware - New USB Bootloader using AVR MCU

![VusbPro](https://github.com/flyandancexo/VusbPro/assets/66555404/f9632b5e-6cfe-4b00-8810-46677e1fc631)

My improvised VusbPro development setup: World faster programmer FDxICSP connected to a mini-heart 2 a Vusb compatible board and 6 wires soldering to a ICSP header connected to a tiny board, the world smallest MCU development board 20mm in diameter, not shown. 

## What is Vusb?
V-usb is a pure software implementation of a USB 1.1 device targeting an AVR MCU. Most modern MCUs already have an internal module for communicating via USB, but most 8-bit AVR MCUs don't. With Vusb, it's possible to turn most AVR MCUs into a semi-USB 1.1 compatible device. It's not a perfect solution, nor is it fast, but being USB semi-compatible means it can be a great USB development tool. The popular micronucleus and USBasp are based on Vusb, but unfortunately, those dummies didn't contribute anything on the Vusb project. 

The V-usb project itself was under maintenance for about a decade, and over that period, different projects had been created with different levels of incomprehensible code and incompetence. VusbPro is an attempt to fix it, making V-usb more accessible by rewriting everything and adding more good comments and more intuitive examples. V-usb is a very under-rated project and it can be very useful if it can be easily used by hobbyist and professional alike. 

Beside rewriting all the code from V-usb and creating few new project examples, the snail slow USBasp will be updated with a guarantee 10kB/s write and 15kB/s read speed, and the USBaspbootloader will be updated to make it more user-friendly, and with auto-upload capability. And of course, few extremely high quality development boards will be developed for it.

## What is hurdle?
MCU, low level assembly, high level C, driver and USB are all the very-hard-to-get-over hurdles. I am an expert on all the above, except for the USB. USB stands for Universal Serial Bus. It makes life easier for everyone but the developers. USB is actually fairly complicated, and I have read few books on it over the past few months and still not 100% sure for everything, but it's more than enough to start this project. USB shouldn't be that complicated. It's the exchange of data between a device and a host. It should be very easy and intuitive, so that is the absolute main goal and objective for VusbPro. The Pro here doesn't not stand for professional, but it's proficient and productive.


# The Plan
- Rewrite Vusb to VusbPro 1.0 (80% DONE)
- Create a beta USBasp firmware based on VusbPro 1.0
- Enhance the algorithms from VusbPro 1.0 to VusbPro 2.0
- Create a final USBasp firmware with VusbPro 2.0
- Create a USB bootloader using VusbPro 2.0
- Create more example projects using VusbPro 2.0
- Create few good development boards for VusbPro 2.0

## Stage 1
- IAR dependent code removed, so VusbPro only works with AVR-GCC compiler
- Unnecessarily code removed, code rearranged in an orderly fashion
- Outdated AVR code removed and/or replaced
- Outdated C code fixed, improved code style
- Data structure renamed back to standard
- PS. My default tab style is 2-space as a tab

The first stage is to clean up the source code in a simplified and easy to read format with good indentations and good comments. Good commenting is the the difference between good code and bad code, so a lot of the effort are spent on rewriting the old comments and adding more comments. IAR compiler option is removed to simplify the source code, and for the fact that I don't use it and it's a paid product. VusbPro 1.0 focuses more on backward compatibility with Vusb, simplification and clarification for moving forward.

## Vusb Decoded

The source code is freely available for anyone to read, but unfortunately, it's very badly written and even more badly documented, and that is exactly why most projects don't even dare to modify the so called driver code. With the rewritten VusbPro, it's clear how the driver works, but you still must be proficient in advanced C programming language namely pointer to grasp how this code actually works. Because it's also written with very bad embedded MCU code; Trying to understand that part could also be with great difficulties for people not fully understand how the MCU works. For now, we focus on the **big picture**. 

Normally, a function is defined first, then it can be called in a project for doing things, but with Vusb, it's the opposite or doing thing backward. The user needs to write the implementations for pre-defined functions for the driver to be called upon. 

```
These functions are assembly functions, and don't need to be bothered with:
extern uint16_t usbMeasureFrameLength(void);
extern uint16_t usbCrc16(uint16_t data, uint8_t len);
extern uint16_t usbCrc16Append(uint16_t data, uint8_t len);
ISR(USB_INTR_VECTOR); //This is the interrupt function
```

There are a lot of C functions, but some are macros. The most important entry point is usbFunctionSetup(); and here is the simplified connections among different functions. This is a typical example in semi-pseudo code. 

```
//Hidden functions
void usbPoll(void){
	usbProcessRx( (usbRxBuf + USB_BUFSIZE + 1 - usbInputBufOffset), len);
}

void usbProcessRx( uint8_t *data, uint8_t len ){
	usbRequest_t *rq = (void *)data;
	replyLen = usbFunctionSetup(data);
}

//New project - Starts here
uint8_t usbFunctionSetup(uint8_t data[8]){ 
	usbRequest_t *rq = (void *)data;
}

int	main(void){
	usbInit();  //Enable interrupt
	
	//Fake disconnect then reconnect USB
	usbDeviceDisconnect();  
	_delay_ms(250 ms);
	usbDeviceConnect();
	
	sei();      //Enable global interrupt
	
	while(1){ usbPoll(); } //Forever loop
}
```

usbRxBuf[] is a raw RX buffer array that contains 1 byte PID, 8 bytes data, 2 bytes CRC; It's an array, but is used by pointer arithmetic as pointer to data. usbRequest_t is an 8 bytes data struct type. The data is being recasted into void* pointer, then converted to an 8-byte data structure for processing. usbFunctionSetup() returns either an uint8_t or uint16_t number, and this is the reply length for the usbProcessRx(); usbProcessRx() doesn't return anything, so the data are passed through it via the pointer. It essentially receives and sends data via the interrupt function. usbPoll() is not really doing the polling on the USB because the USB data pins are monitored by external interrupt, so what usbPoll() does is to check if new data has arrived or not, and then call usbProcessRx() to retrieve the data. This has been over-simplified, and yet still is very complicated, but it really doesn't have to be like that. It really doesn't. 


![VusbProNote](https://github.com/flyandancexo/VusbPro/assets/66555404/40102138-ee39-4917-ab51-7f64cb7bbd07)

This is a long and complicated endeavor, so any support is very welcomed. Do donate whatever amount that you are comfortable with. 

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://paypal.me/flyandance?country.x=US&locale.x=en_US)
