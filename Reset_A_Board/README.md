# HEX file required to reset the board can be found from the below linked page:
Please go [Here](https://rules.ectf.mitre.org/2025/getting_started/daplink.html) and click on the link called "*Download the DAPLink interface firmware here: DAPLink_NoReset.hex*".

## Steps:
1. Download the HEX file from the above link.
2. Keep holding SW5 button of the board, insert the cable connecting the board to your PC through USB interface, keep holding the button.
3. Once Red Led appears on the board, you can leave the button. Now you are in maintenance mode.
4. Drag and drop the HEX file downloaded earlier into the drive named MAINTENANCE on your PC.
5. After some time the MAINTENANCE drive will be gone and the DAPLINK drive will have appeared.
6. Board has been RESET.

## Setup the bootloader for eCTF 2025 now:
1. Download the insecure.bin file if you don't already have it from here: [Link](https://rules.ectf.mitre.org/_downloads/6176d2473ff417b11a757dd7967b19c2/insecure.bin)
2. Drag and drop it into the DAPLINK drive which appeared. After sometime of loading the drive will disappear and reappear.
3. Board has been bootloaded with the insecure.bin
