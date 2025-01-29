# Stress Tester Tool Notes

The script is a stress test for an encoder and decoder system used in the MITRE eCTF 2025 competition. It accepts command-line arguments and runs performance tests on encoding and decoding data frames.

## General Command-Line Args:

| Argument              | Type             | Default        | Description                              |
| :-------------------- | :--------------- | :------------- | :--------------------------------------- |
| `--frame-size` / `-f` | Integer          | `64`           | Size of each frame in bytes              |
| `--test-size` / `-t`  | Integer          | `100000000`    | Total number of bytes to process         |
| `--channels` / `-c`   | List of Integers | `[0, 1, 2, 3]` | List of channels to randomly choose from |

## Inputs for Encoding Test (`encode` subcommand):

| Argument     | Type             | Required?      | Description                                           |
| :----------- | :--------------- | :------------- | :---------------------------------------------------- |
| `secrets`    | Binary File      | ✅ Yes         | Path to a secrets file (used by the encoder)          |
| `--dump`     | JSON File        | ❌ No          | Optional output file for storing encoded frames       |

