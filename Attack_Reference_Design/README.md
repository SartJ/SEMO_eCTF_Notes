# Attacker Flag Tutorial

#### Author: [Alimus Sifar](https://github.com/AlimusSifar)

**Download the `attack_package.zip` from here:** [Link](https://github.com/SartJ/SEMO_eCTF_Notes/blob/main/Attack_Reference_Design/attack_package.zip)

**Extract the contents of `attack_package.zip` file** in the directory where `.venv` is located. 


Example:

```bash
<project_root>\
├── .venv\
├── attack_package\
│   ├── attacker.bin
│   ├── sub_1.bin
│   └── ...
├── decoder\
├── design\
├── tools\
└── ...
```

> NOTE: The Attack Board will be in update mode from the start. Verify this by looking at the blue LED blinking. "Once it flashes, it may not flash again.**

## Contents included in `attack_package.zip`

### Attacker Decoder

- _image_: attacker.bin
- _Decoder ID_: 0xdeadbeef (fixed)

### Subscriptions

- _sub_1.bin_: Subsscription to channel 1 for attacker decoder.
- _sub_2.bin_: Subsscription to channel 2 for attacker decoder.
- _sub_3.bin_: Subsscription to channel 3 for unknown decoder.

### Frame Recordings

- _frame_playback.json_: JSON file of a list of recorded frames from channel 1.

## Flashing the Attacker Decoder

Copy `attacker.bin` file and paste it in the `DAPLINK` drive to flash the attacker decoder. If successful, the board will turn on the green LED.

At this moment, the board will have no subscriptions. Verify that by running:
```bash
.venv\Scripts\activate  # Activate the python virtual environment
```

```bash
python -m ectf25.tv.list COM3
```

## Subscribing to Channels

Subscribe to the provided subscription files by running:

```bash
python -m ectf25.tv.subscribe COM3 attack_package\sub_1.bin
```

```bash
python -m ectf25.tv.subscribe COM3 attack_package\sub_2.bin
```

```bash
python -m ectf25.tv.subscribe COM3 attack_package\sub_3.bin
```

Verify the subscriptions by running:

```bash
python -m ectf25.tv.list COM3
```
> NOTE: Double-check the correct port (e.g., COM3, COM6) is used when subscribing to channels.


## Connecting to Satellite

The remote Satellite information was provided in the Slack [#announcements](https://mitre-ectf.slack.com/archives/C085Z46DPRN/p1738871765312639) channel.

```bash
Satellite IP: 3.81.51.207
Channel 0 port: 2000
Channel 1 port: 2001
Channel 2 port: 2002
Channel 3 port: 2003
CHannel 4 port: 2004
```

Test connection to Satellite's Channel 0:

```bash
python -m ectf25.tv.run 3.81.51.207 2000 COM3
```

## Capturing the flags

Three flags are hidden in the broadcasted frames of channel 2, 3, and 4. Another flag is decoded from the provided `frame_playback.json` file.

Format of the flags (e.g. Channel 1):

```bash
noflagon
thischan
^ flag ^
00062e0d
3dd7dac7
^ time ^
e78ec691
94ba3474
```

### Channel 2 Flag

```bash
python -m ectf25.tv.run 3.81.51.207 2002 COM6
```

### Channel 3 Flag

```bash
python -m ectf25.tv.run 3.81.51.207 2003 COM6
```

### Channel 3 Flag

```bash
python -m ectf25.tv.run 3.81.51.207 2004 COM6
```

### Channel 1 Flag (Manual decoding)

The Channel 1 flag is decoded from the provided `frame_playback.json` encoded frames. After analyzing how the TV module collects frames from the Satellite, we can crack the decoding method.

Sample data from `frame_playback.json`:

```json
{
  "channel": 1,
  "timestamp": 2009135666390588,
  "encoded": "010000003c66b3594c230700396237376666346636356630343962375e20666c6167205e303030373233346335396233363633635e2074696d65205e39313936626564626632656631313336"
}
```
To decode Channel 1’s flag, we need to analyze the encoded frame from frame_playback.json. Here's how to proceed:


- Copy the encoded frame and start tracing from `tools\ectf25\tv\run.py`.
- Navigate to line 130 in `tools\ectf25\utils\decoder.py`. Replace the encoded frame in bytes for decoding

```python
def decode(self, frame: bytes) -> bytes:
    # force the frame to be our encoded frame.
    frame = bytes.fromhex("paste the encoded frame here.")
    # send decode message
    msg = Message(Opcode.DECODE, frame)
```

Then, connect to any channel, you will decode the frame.

```bash
python -m ectf25.tv.run 3.81.51.207 2001 COM3
```

---

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand," Martin Fowler
