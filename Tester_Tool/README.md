# Notes on the `tester.py` tool

This is a Host Tool. Link: [Click Here](https://github.com/mitre-cyber-academy/2025-ectf-insecure-example/tree/release?tab=readme-ov-file#tester-tool)

## Importance of the tester tool

To run all of the infrastructure, you will need to first start the uplink. Then, in a separate terminal window, start the satellite. Finally, start a TV for every decoder being tested while the satellite is running.

*However,* The Tester tool can be used to test frame decoding functionality without running the end to end infrastructure.

## Example Command:
```
python -m ectf25.utils.tester --port COM12 -s secrets\secrets.json rand -c 1 -f 64
```
### Example Command Breakdown:
- Replace the port number with yours in place of `COM12`
- `-s` denotes the path to the global secret file
- `rand` is one of the **3** possible subcommands. It generates a random frame.
- `-c` denotes the ID of the channel we are testing.
- `-f` denotes the size of the frame. By default it is set to `64`.

The script accepts a variety of command-line arguments to control its behavior.

For example, the above command keeps generating frames forever, since we did not specify any number of frames for it to generate. We could do so using the `-n` argument. Below example produces 3 random test frames.
```
python -m ectf25.utils.tester --port COM12 -s secrets\secrets.json rand -c 1 -f 64 -n 3
```


## General Arguments:
* `--secrets` (`-s`): Required. A binary file containing secrets for encoding/decoding.
* `--port` (`-p`): The serial port to communicate with the decoder. Required unless `--stub-decoder` is specified.
* `--delay` (`-d`): Delay (in seconds) after each frame is decoded.
* `--perf`: Display performance statistics.
* `--stub-encoder`: Stub out the encoder (pass frames directly to the decoder).
* `--stub-decoder`: Stub out the decoder (print encoded frames instead of decoding).
* `--dump-raw`: Path to a file to dump raw frames.
* `--dump-encoded`: Path to a file to dump encoded frames.
* `--dump-decoded`: Path to a file to dump decoded frames.

## Subcommands:
* `stdin`: Read frames from stdin.
* `rand`: Generate random frames.
* `json`: Read frames from a JSON file.

## Subcommand-Specific Inputs

Each *subcommand* accepts additional *arguments*.

* `stdin` Subcommand:
  * Expects input in the format: `channel(int),frame(str),timestamp(int)` via `stdin`.
* `rand` Subcommand:
  * `--ascii` (`-a`): Generate ASCII-printable frames only.
  * `--num-frames` (`-n`): Number of frames to generate (default: infinite).
  * `--channels` (`-c`): List of channel numbers to randomly choose from (e.g., 0, 1, 2, 3).
  * `--frame-size` (`-f`): Frame size in bytes (default: 64).
* `json` Subcommand:
  * `file`: JSON file path with frames structured as `[[channel, frame, timestamp], ...]`.
  * `--real-ts`: Replace timestamps in the JSON with real/live timestamps.
  * `--loop`: Loop through the JSON file indefinitely.
