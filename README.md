# SEMO_eCTF_2025_Notes

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

OS Used: Windows

For Linux Users: [Link](https://github.com/SartJ/SEMO_eCTF_Notes/tree/updated-readme/LInux_Documentation)

_Okay, let's start_

### Flashing the eCTF Bootloader for the first time:

Follow the below link to see the steps:
[LINK](https://github.com/SartJ/SEMO_eCTF_Notes/tree/main/Reset_A_Board)

Next, on your windows host machine at any suitable location follow the next steps.

### Clone the Reference Design

```
git clone https://github.com/mitre-cyber-academy/2025-ectf-insecure-example
```

### Creating the Python Virtual Environment

We used Powershell for the next commands.

```
python -m venv .venv --prompt ectf-example
```

^ sets up the environment

```
.\.venv\Scripts\Activate.ps1
```

^ this command activates the virtual environment

```
python -m pip install .\tools\
```

```
python -m pip install -e .\design\
```

^ last two command installs all the necessary dependencies

### Locating the Port

1. Go to Device Manager (Press Windows + R; type 'devmgmt.msc'; hit enter.
2. Under Ports (COM & LPT), you will find the COMxx, 'xx' is the number helps identify the port. Remember this.
   ![image](https://github.com/user-attachments/assets/c2841ae8-78bd-450b-9d84-84d962eda18a)

### Install Docker

1. SignUp, choose a free plan, and download to install docker.
2. After docker is installed Sign into it in the Desktop Docker software.
3. If using docker for the first time there will be no containers listed. But below is how docker desktop will look like at this point.
   ![image](https://github.com/user-attachments/assets/b6c42c93-2955-4500-aecd-45f78dd8af7c)
4. Then, keep it minimized.

### Creating a Deployment

Creating shared secrets which will be used by both the Encoder and Decoder:

```
python -m ectf25_design.gen_secrets global.secrets 1 3 4
```

Next we will create the build environment with docker build and the Decoder with an ID of 0xdeadbeef with docker run:

```
cd ./decoder
docker build -t decoder .
docker run --rm -v .\build_out:/out -v .\:/decoder -e DECODER_ID=0xdeadbeef decoder
```

Building this image will take some time, roughly about 10 minutes.

If you get errors in building the docker image, try logging in docker using the command below & then repeat the build process.

```
docker login -u **Your Username/Email** -p **Your Password**
```

### Docker Interactive Terminal Set-up Commands
```
cd .\decoder\

docker build -t decoder .

docker run -v .\build_out:/out -v .\:/decoder -it decoder

make release

cd ..
```

### Generating a Subscription Update

Next, we will create a subscription update. We must change our location to the project root folder so we have access to the secrets. To do so and generate a new subscription, enter the commands:

```
cd <design_root>
python -m ectf25_design.gen_subscription secrets/secrets.json subscription.bin 0xDEADBEEF 32 128 1
```

This will create a binary file, that can be applied to the the Decoder we created previously (ID 0xDEADBEEF) to subscribe to channel one from the timestamp 32 to the timestamp 128.

### Flashing Decoder Firmware

Now we flash the board:

```
python -m ectf25.utils.flash .\decoder\build_out\max78000.bin COM3
```
```ps
python -m ectf25.utils.flash .\decoder\build\max78000.bin COM3
```

Replace the section labeled ‘COM3’ with the port you located earlier from Device Manager. Then the board is flashed.

### Next, try out the `Host Tools`:

[Follow link](https://github.com/mitre-cyber-academy/2025-ectf-insecure-example/?tab=readme-ov-file#host-tools-1)

### Check how to utilize the `tester.py` tool for testing the design:

[Link](https://github.com/SartJ/SEMO_eCTF_Notes/blob/main/Tester_Tool/README.md)

### Check how to utilize the `stress_test.py` tool:

[Link](https://github.com/SartJ/SEMO_eCTF_Notes/tree/main/Stress_Tester_Tool)

### Enable WolfSSL, in a few steps:

[Link](https://github.com/SartJ/SEMO_eCTF_Notes/tree/main/WolfSSL_Notes)

### Notes for Reference Design Attack:
[Link](https://github.com/SartJ/SEMO_eCTF_Notes/tree/main/Attack_Reference_Design)
