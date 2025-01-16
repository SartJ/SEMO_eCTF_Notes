# Dealing with Firmware update / boot on boards without a clean slate 

I was able to fix my previous FTHR board. I had some example MAXIM codes running on it, so Flashing the eCTF Bootloader was being a hassle at first. Then I realized a few things:

It is a certain way we need to press and a certain time we need to release the SW5 button. That is Important.

Steps:
1. Hold SW5, remove the usb from the usb-port. Keep holding, insert it back to the usb port. As soon as you see a green LED flash leave pressing the SW5 button. 
[If we keep pressing the SW5 button it enters the MAINTENANCE mode and Red LED Flashes, that is what we Do Not Want. In MAINTENANCE mode instead of a drive named DAPLINK we will get a drive named MAINTENANCE. We don't need that now].

2. Now we will see a drive named DAPLINK, where we need to drag and drop the '[insecure.bin](https://rules.ectf.mitre.org/_downloads/6176d2473ff417b11a757dd7967b19c2/insecure.bin)' file. There will be a progress bar, and the daplink drive will close. Then if we open it again, there will be no FAIL report hopefully. That means we succeeded. 

3. Flashing the eCTF Bootloader is done, continue with booting the design reference. Following steps given [here](https://github.com/SartJ/SEMO_eCTF_Notes/tree/main).
