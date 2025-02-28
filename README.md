# Raspberry Pi OS built with Yocto Project

- SD CARD image is attached with this same github repository named 

    ``` core-image-minimal-raspberrypi4-20250228183041.rootfs.rpi-sdimg ```
- Layers used are poky ( branch kirkstone ), meta-raspberrypi ( branch  kirkstone ), meta-openembedded ( branch kirkstone )
- Also to create an application which runs on boot, I have made a custom layer which prints HelloWorld on the screen when booting finishes.
- Structure of the application layer during build was
    * meta-exampleapp
        * conf
            * layer.conf
        * recipes-exampleapp
            * HelloWorld
                * HelloWorld_1.0.bb
    * This bb layers has to be added in build/conf/bblayers.conf
- Changed the local.conf 
	* MACHINE ??= "raspberrypi4"(for 32 bit system), ENABLE_UART = "1", 
	* EXTRA_IMAGE_FEATURES:append  = " ssh-server-dropbear"(for ssh functionality), 
	* PREFERRED_PROVIDER_virtual/kernel ?= "linux-raspberrypi"
	* PREFERRED_VERSION_linux-raspberrypi ?= "5.15%" (to use the exact 5.15 linux-kernel version)
- For tickless CPU operation, kernel configuration needs to be set. The file for config is situated ``` build/tmp/work/raspberrypi4-poky-linux-gnueabi/linux-raspberrypi/1_5.15.92+gitAUTOINC+509f4b9d68_14b35093ca-r0/defconfig ``` at the defconfig.
    * CONFIG_NO_HZ=y

### Problems occured during build
* One Major problem occured was because of college firewall issues. When you execute command ```bitbake core-image-minimal```, it starts downloading all the kernel elements, bash, curl, python, etc which is needed to build the OS.
* During this time linux-kernel-5.15.92 from the git repository was failing everytime due to firewall and speed issues. To make it work vpn was used with another high speed internet connection. Ref : https://lists.yoctoproject.org/g/yocto/topic/raspberry_pi_do_fetch_failure/61272128 

    ``` linux-raspberrypi-1_5.15.92+gitAUTOINC+509f4b9d68_14b35093ca-r0 do_fetch: Failed to fetch URL git://github.com/raspberrypi/linux.git;name=machine;branch=rpi-5.15.y;protocol ```


* This process of pulling the linux kernel took major time of the build. Also each yocto build needs a high end PC to compile.