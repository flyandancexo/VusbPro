# VusbPro - Enhanced V-USB, a software-based USB device
### High Speed USBasp firmware - New USB Bootloader using AVR MCU

![VusbPro](https://github.com/flyandancexo/VusbPro/assets/66555404/f9632b5e-6cfe-4b00-8810-46677e1fc631)

The V-usb project was under maintenance for about a decade, and over that period, different projects had been created with different levels of incomprehensible code and incompetence. VusbPro is an attempt to fix it, making V-usb more accessible by rewriting everything and adding more good comments and more intuitive examples. V-usb is a very under-rated project and it can be very useful if it can be easily used by hobbyist and professional alike. 

Beside rewriting all the code from V-usb and creating few new project examples, the snail slow USBasp will be updated with a guarantee 10kB/s write and 15kB/s read speed, and the USBaspbootloader will be updated to make it more user-friendly, and with auto-upload capability. 

# The Plan
- Rewrite Vusb to VusbPro 1.0
- Create a beta USBasp firmware based on VusbPro 1.0
- Enhance the algorithms from VusbPro 1.0 to VusbPro 2.0
- Create a final USBasp firmware with VusbPro 2.0
- Create a USB bootloader using VusbPro 2.0
- Create more example projects using VusbPro 2.0

## Stage 1
- IAR dependent code removed, so VusbPro only works with AVR-GCC compiler
- Unnecessarily code removed, code rearranged in an orderly fashion
- Outdated AVR code removed and/or replaced, improved code style
- C Functions and macros renamed and rewritten
- Data structure renamed back to standard and cleaned up

The first stage is to clean up the source code in a simplified and easy to read format with good indentation and good comment. Good commenting is the the difference between good code and bad code, so a lot of the effort are spent on rewriting the comments and adding more comment. IAR compiler option is removed to simplify the source code, and for the fact that I don't use it and it's a paid product.

![VusbProNote](https://github.com/flyandancexo/VusbPro/assets/66555404/40102138-ee39-4917-ab51-7f64cb7bbd07)

This is a long and complicated endeavour, so any support is very welcomed. Do donate whatever amount that you are comfortable with. 

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://paypal.me/flyandance?country.x=US&locale.x=en_US)
