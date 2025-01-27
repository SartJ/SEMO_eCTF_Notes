# SEMO_eCTF_Notes

## Reference Design Boot Details:

First we made sure that our Windows machine had the following 3 tools installed:

1. Git
2. Python 3.11+
3. Docker (Installation steps can be found below)

Check if you already have them or not, as below:

![image](https://github.com/user-attachments/assets/f50216fa-4eef-4aed-8a79-7d8c65598225)

### Some important links:

- Specification Home Page (eCTF 2025): https://rules.ectf.mitre.org/2025/index.html
- Reference Design boot description: https://rules.ectf.mitre.org/2025/getting_started/boot_reference.html
- Reference design github link: https://github.com/mitre-cyber-academy/2025-ectf-insecure-example/

OS Used: Linux

### Flashing the eCTF Bootloader in linux:

Follow the link to download DAPLINK which might be required for debugging & download the insecure bin file.
[Link](https://rules.ectf.mitre.org/2025/getting_started/boot_reference.html#id1)

After that follow the steps listed below :

### Clone the assignment

```
git clone https://github.com/mitre-cyber-academy/2025-ectf-insecure-example/
```

### Follow the steps below to use ECTF Tools

```
# Create a virtual environment in the root of the design
cd <directory of >
python -m venv .venv --prompt ectf-example

# Enable virtual environment
. ./.venv/bin/activate

# Install the host tools
python -m pip install ./tools/

# Install the host design elements as an editable module
python -m pip install -e ./design/
```

### Locating the Serial Port

```
# Run the command below first before plugging in the firmware
ls /dev/tty*

# After that plug in the firmware and see the updated list using the same command
ls /dev/tty*

# New file might have been added to it of format
/dev/ttyACM0
/dev/ttyUSB0
/dev/tty.usbmodem11302
```

### Generate Secrets for both encoder & decoder

```
mkdir secrets

# Generating secrets for channels 1,3 & 4
python -m ectf25_design.gen_secrets secrets/secrets.json 1 3 4
```

### Build Docker for Decoder

```
# From the root folder of the project go to decoder directory
cd ./decoder
docker build -t decoder .
docker run --rm -v ./build_out:/out -v ./:/decoder -v ./../secrets:/secrets -e DECODER_ID=0xdeadbeef decoder
```

### Generate a subscription bin file for decoder

```
# Change to root directory
cd <design_root>
python -m ectf25_design.gen_subscription secrets/secrets.json subscription.bin 0xDEADBEEF 32 128 1

```

### Flash the decoder firmware

```
# Change the name of serial port accordingly
python -m ectf25.utils.flash ./decoder/build_out/max78000.bin /dev/ttyACM0
```

### For issues with permission when running the flash command

```
# Temporary Solution
# Temporarily you can add the permissions to read and write
sudo chmod 666 /dev/ttyACM0

# Additionally only dialout group users are allowed to access serial ports
# So add current user to the group
sudo usermod -aG dialout $USER

```

### Next, try out the `Host Tools`:

[Follow link](https://github.com/mitre-cyber-academy/2025-ectf-insecure-example/?tab=readme-ov-file#host-tools-1)

### Check how to utilize the `tester.py` too for testing the design:

[Link](https://github.com/SartJ/SEMO_eCTF_Notes/blob/main/Tester_Tool/README.md)
