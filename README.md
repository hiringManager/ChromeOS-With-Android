

# ChromeOS-With-Android

- [Here's the Original Guide Before I Make Fun of it](#heres-the-original-guide-before-i-make-fun-of-it)
- [Installation](#installation)
    - [Download recovery Image (Usually Samus):](#download-recovery-image-usually-samus)
    - [Download Latest Brunch and Extract](#download-latest-brunch-and-extract)
    - [Throw Recovery Image and extracted Brunch into to a folder named ChromeOS](#throw-recovery-image-and-extracted-brunch-into-to-a-folder-named-chromeos)
    - [Boot a Linux USB](#boot-a-linux-usb)
    - [Figure out which Drive you're going to format. You can just use a usb / sd card to test.](#figure-out-which-drive-youre-going-to-format-you-can-just-use-a-usb--sd-card-to-test)
    - [Install Dependencies](#install-dependencies)
    - [Install To Drive](#install-to-drive)
- [Advice](#advice)

This is a simplified method of installing ChromeOS on most Computers - and with Android support. It works incredibly well, even from a flash drive or something, since ChromeOS is based off of Gentoo. In fact, running it from a usb2 isn't significantly worse than an ssd after the long **usb2-**flavored boot - since so much of the OS is cached to ram.

## Here's the Original Guide Before I Make Fun of it

https://beebom.com/how-install-chrome-os-on-pc/

He makes the process needlessly complicated, and also does something that I find terribly annoying. He advertises in his 1-line shell script -- and that's just bad manners. Also he ran **figlet as sudo**, and I can't respect a man who does that. Additionally the 'magic script' he talks about is literally the one code block that I threw in this.

Thanks to him I can do this with crouton easily though lol.

## Installation 

### Download recovery Image (Usually Samus):

    https://cros-updates-serving.appspot.com/
    https://cros.tech/

- From Original Guide
    “rammus” is the recommended image for devices with **4th generation Intel CPU** and newer.**
    “samus” is the recommended image for devices with **3rd generation Intel CPU** and older.**
    “zork” is the image to use for **AMD Ryzen 3XXX**.**
    “grunt” is the image to use for **AMD Stoney Ridge**.**
    Note: I've actually done this with older images successfully. If it doesn't work for you, then just try a more reasonable one for your hardware.


### Download Latest Brunch and Extract

https://github.com/sebanc/brunch/releases/

### Throw Recovery Image and extracted Brunch into to a folder named ChromeOS

-   Just to keep things orderly

### Boot a Linux USB 

    - Does not have to be Debian. Same thing on rpm/pacman.
    - Doesn't matter what distro, you just need to be able to pull in the dependencies - 'pv' and 'cgpt'.
    - You can also do this in wsl, but this is easier.

### Figure out which Drive you're going to format. You can just use a usb / sd card to test.

```
sudo fdisk -l 
```

### Install Dependencies

``` 
sudo apt-get update
sudo apt-get install pv
sudo apt-get install cgpt

- In the next step, rename 'samus_recovery' to rammus or whatever. And choose the right block device. It's harder to just use a partition to install, so I left it out.*

- Again, this will format whichever block device you chose so don't blame me if you formatted your disk. Look at the output from fdisk
```

### Install To Drive

```
sudo bash ./chromeos-install.sh -src samus_recovery.bin -dst /dev/sdx
```

- Wait like 10 minutes for it to compile.
- Boot from USB and you're done. It has to rebuild the initramfs or something on first boot but after that, you'll have a normal boot time.

+ Problems? 

- Is secure boot disabled? UEFI works easier than legacy btw -- **Read the brunch git for additional information.**



## Advice

- **Don't resize** it after creation. It's locked to a checksum, so if you resize it, you'll have to reinstall. If you have 8gb of ram, just toss it on a 32 gb usb and you're good. It'll cache most of the OS anyway after like 5 mins of being booted.

- The Guide says don't use your actual **Google Account**, but I don't buy that. There is too much plausible deniability, and they acquired Cloudready, and will most likely be pushing this exact method soon. Your choice though.

- This is **missing some of the TPM security** but it's still sandboxed ofc. People say it's less secure, but are you going to be reasonably hackable on Gentoo.ChromeOS? No. Plus tpm security has a history of **not being identifiably more secure**, so whatever.

- Don't let windows touch any of the partitions. It'll break the checksum I mentioned.  

- RTFM, this will not work with anything prior to Gen 1 Intel, (not sure when the cutoff is for AMD). For example I couldn't install this on a 2007 imac, even though virtualization is supported. The necessary cpu features aren't there, and it will not be patched in. Additionally, Cloudready has dropped support for older hardware recently, so you may find yourself having to switch over to Linux for your older boxes. 
