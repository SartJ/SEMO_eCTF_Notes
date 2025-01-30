# Configure the decoder system with WolfSSL Library


## Edit the `decoder/project.mk` file

We uncomment and comment in the following way:
```ini
# Disable Crypto Example
# CRYPTO_EXAMPLE=0

# Enable Crypto Example
CRYPTO_EXAMPLE=1
```

## Cloning the WolfSSL Library's GitHub Repo

- Make sure you are in the `decoder` directory on your terminal, Example: `"C:\...\2025-ectf-insecure-example-semo\decoder>"`
- Then, use the following command:
  ```sh
  git clone https://github.com/wolfssl/wolfssl 
  ```

## Delete old Docker stuffs

Old docker did not have WolfSSL, we will delete the old docker Container (*if exists*), Image (**must delete**), Builds (*optional*).

![image](https://github.com/user-attachments/assets/da64551b-b569-4e17-a513-7614a2e1d129)

## Run the following commands

If you are not on your "decoder" directory, cd into it, and run the following commands once more:

```sh
docker build -t decoder .
docker run --rm -v .\build_out:/out -v .\:/decoder -v .\..\secrets:/secrets -e DECODER_ID=0xdeadbeef decoder
```

## Update firmware
- Plug in the decoder
- Paste the `insecure.bin` file into the DAPLINK drive
- Use the below command to flash the new firmware with WolfSSL enabled:
```sh
python -m ectf25.utils.flash .\decoder\build_out\max78000.bin COM3
```

## Verify

Use the following command to verify:

```sh
python -m ectf25.tv.list COM3
```

## Without WolfSSL enabled
![image](https://github.com/user-attachments/assets/7ff13ff8-71de-4ad4-94d0-3050da1d4989)

## With WolfSSL enabled (expected)
![image](https://github.com/user-attachments/assets/a4b2cc98-cd50-4180-8769-bcce0b8ab831)

