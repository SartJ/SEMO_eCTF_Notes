# Configure the decoder system with WolfSSL Library


## Step 1: Edit the `decoder/project.mk` file

We uncomment and comment in the following way:
```ini
# Disable Crypto Example
# CRYPTO_EXAMPLE=0

# Enable Crypto Example
CRYPTO_EXAMPLE=1
```

## Step 2: Cloning the WolfSSL Library's GitHub Repo

- Make sure you are in the `decoder` directory on your terminal, Exampe: `"C:\...\2025-ectf-insecure-example-semo\decoder>"`
- Then, use the following command:
  ```sh
  git clone https://github.com/wolfssl/wolfssl 
  ```

## Delete old Docker stuffs

Old docker did not have WolfSSL, we will delete the old docker Container, Image, Builds (optional).

![image](https://github.com/user-attachments/assets/57a6c3b8-7acd-48a0-a8cb-e35f20c423d5)

## Run the following commands

If you are not on your "decoder" directory, cd into it, and run the following commands once more:

```sh
docker build -t decoder .
docker run --rm -v .\build_out:/out -v .\:/decoder -v .\..\secrets:/secrets -e DECODER_ID=0xdeadbeef decoder
```
