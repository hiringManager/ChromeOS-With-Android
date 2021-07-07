# ChromeOS-With-Android

This is a simplified method of installing ChromeOS on most Computers - and with Android support.

### Here's the Original Guide

https://beebom.com/how-install-chrome-os-on-pc/

He makes the process needlessly complicated, and also does something that I find terribly annoying. He advertises in his 1-line shell script and that's just bad manners. Also he ran **figlet as sudo**, and I can't respect a man who does that.

Thanks to him I can do this with crouton easily though.

#### How to Install 

* Download recovery Image (Usually Samus):
    https://cros-updates-serving.appspot.com/
    https://cros.tech/

// From Original Guide
**“rammus” is the recommended image for devices with 4th generation Intel CPU and newer.**
**“samus” is the recommended image for devices with 3rd generation Intel CPU and older.**
**“zork” is the image to use for AMD Ryzen 3XXX.**
**“grunt” is the image to use for AMD Stoney Ridge.**
// Note: I've actually done this with older images successfully. If it doesn't work for you, then just try a more reasonable one for your hardware.


* Download Latest Brunch and Extract

https://github.com/sebanc/brunch/releases/

* Throw Recovery Image and extracted Brunch into to a folder named ChromeOS

* Boot a Linux USB 
    * Does not have to be Debian. Same thing on rpm/pacman.
    * Doesn't matter what distro, you just need to be able to pull in the dependencies - 'pv' and 'cgpt'.
    * You can also do this in wsl, but this is easier.
* Figure out which Drive you're going to format. You can just use a usb / sd card to test.

``` sudo fdisk -l ``` 

* Install Dependencies

``` 
sudo apt-get update
sudo apt-get install pv
sudo apt-get install cgpt
```

* In the next step, rename 'samus_recovery' to rammus or whatever. **And choose the right block device.** It's harder to just use a partition to install, so I left it out.*
* Again, this will format whichever block device you chose so don't blame me if you formatted your disk. **Look at the output from fdisk**

* Begin Install 
    ```sudo bash ./chromeos-install.sh -src samus_recovery.bin -dst /dev/sdx ```
* Wait like 10 minutes for it to compile.
* Boot from USB and you're done. And it runs amazingly, not gonna lie.
    Problems? Secure boot disabled? UEFI works easier than legacy. **Read the brunch git for additional information.**

## Tips:

* Don't resize it after creation. It's locked to a checksum, so if you resize it, you'll have to reinstall. If you have 8gb of ram, just toss it on a 32 gb usb and you're good. It'll cache most of the OS anyway after like 5 mins of being booted.
* The Guide says don't use your actual Google Account, but I don't buy that. There is too much plausible deniability, and they acquired Cloudready, and will most likely be pushing this exact method soon. 
* This is missing some of the TPM security but it's still sandboxed ofc. People say it's less secure, but are you going to be reasonably hackable on Gentoo.ChromeOS? No. Plus tpm security has a history of not being identifiably better, so whatever.
