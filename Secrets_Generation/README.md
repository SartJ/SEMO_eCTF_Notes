# Generating Random Secrets

![Project GIF](placeholder.gif)

## Author

**[The Dark Knight]**(https://www.linkedin.com/in/uzairhussain0/) Savior Of R3DH4Ck5 (SEMO MITRE eCTF Team)

![Avatar](https://media.tenor.com/NngS_U0rYAkAAAAM/batman.gif)

## Description

File of interest : 2025-ectf-insecure-example\design\ectf25_design>gen_secrets.py

Adding randomness to secret generation

## Importing

At the top of the (gen_secrets.py) file where all the libraries are imported (you can use alias to avoid name conflict)
```sh
# import the package/lib
import secrets as sec

```

this is built in python 3.6 and above,


## Usage
In the (gen_secrets.py) file find gen_secrets() function, commnet out the hardcoded secret value and add the line below to it.

```sh
 secrets = {
        "channels": channels,
       # "some_secrets": "EXAMPLE",
        "some_secrets": sec.token_hex(16),

```


you can change the length of the secret by changing the byte size


