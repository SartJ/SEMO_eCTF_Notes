# Debugging Flag Tutorial

#### Author:
- [Alimus Sifar](https://github.com/AlimusSifar)

## Pre-requisites

- Debug Bootloaders (`gdb_challenge_25.bin`, `gdb_challenge_25.elf`)
  - Download from [Debugger Flag — 2025 eCTF 2025.01.15 documentation](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html)
- Maxim SDK (MSDK)
  - Online Installer [MaximMicrosSDK_win.exe](https://github.com/analogdevicesinc/msdk/releases/download/v2024_10/MaximMicrosSDK_win.exe)
  - Online Installer [MaximMicrosSDK_linux.run](https://github.com/analogdevicesinc/msdk/releases/download/v2024_10/MaximMicrosSDK_linux.run)
  - Online Installer [MaximMicrosSDK_macOS.dmg](https://github.com/analogdevicesinc/msdk/releases/download/v2024_10/MaximMicrosSDK_macOS.dmg)
- Docker running

### Firmware Update

- Connect the **`MAX78000 FTHR`** board to the computer and find the COM port value
- Copy and paste the `insecure.bin` file in the DAPLINK folder (If successful, Blue LED should blink)
- Open CMD/PowerShell/Terminal and activate the python environment, e.g.
  - `.venv\Scripts\activate` (Windows)
  - `.venv/bin/activate` (Linux/MacOS)
- Check if **`ectf25`** is installed by running `pip list`
- Flash the downloaded bootloaders to the device using `python -m ectf25.utils.flash <PATH_TO_BIN_FILE> <DEVICE_PORT>`

Example:

```sh
python -m ectf25.utils.flash .\debugging\gdb_challenge_25.bin COM13
```

> _**NOTE:** Copy `gdb_challenge_25.bin` and `gdb_challenge_25.elf` file and Paste them in `2025-ectf-insecure-example\debugging` directory for simplicity._

- If successful, the board will blink multiple colors.

### MSDK Installation

- Run `MaximMicrosSDK` installer for Windows/Linux/MacOS
- Select **Installation Folder** or keep _default_
- On **Select Components** section, find **MAXIM Microcontrollers** and Uncheck all except the **`MAX78000 Resources`**
- Accept the **License Agreement** and follow through the instructions to complete the installation
  > _**NOTE:** After installation, it will open **VSCode-Maxim** readme. It's not required, so **Cancel** that._

## Testing OpenOCD

Open **CLI** and **cd** to `...\MaximSDK\Tools\OpenOCD` directory and run:

```sh
openocd -s scripts/ -f interface/cmsis-dap.cfg -f target/max78000.cfg -c "bindto 0.0.0.0; init"
```

Successful output should have:

```sh
Info : Listening on port 3333 for gdb connections
Info : Listening on port 6666 for tcl connections
Info : Listening on port 4444 for telnet connections
```

> _**NOTE:** Make sure `Listening on port 3333 for gdb connections` is showing._

### Running GDB in Docker

- Start Docker if it's not running.
- Open **CLI** and **cd** to `2025-ectf-insecure-example` directory and run `docker run --rm -it -p 3333:3333/tcp -v {PATH_TO_DIRECTORY_CONTAINING_TARGET}:/out --workdir=/root --entrypoint /bin/bash {DECODE_IMAGE_NAME} -c "cp -r /out/* /root/ && gdb-multiarch {TARGET}.elf"`
  > _**NOTE:** Replace `{PATH_TO_DIRECTORY_CONTAINING_TARGET}` with the folder where `gdb_challenge_25.elf` is found, `{DECODE_IMAGE_NAME}` with the docker image name, e.g. `decoder`, and `{TARGET}` with `gdb_challenge_25`_

Example:

```sh
docker run --rm -it -p 3333:3333/tcp -v .\debugging:/out --workdir=/root --entrypoint /bin/bash decoder -c "cp -r /out/* /root/ && gdb-multiarch gdb_challenge_25.elf"
```

Successful output should have:

```sh
Reading symbols from gdb_challenge_25.elf...
```

#### Initiate Remote Debugging in GDB CLI

In GDB shell, run:

```sh
target remote host.docker.internal:3333
```

## Finding the Flag

The debugger flag is in the program's `check_flag` function which takes 5 arguments. However, we need to provide 3 arguments (value1, value2, value3) and keep 2 arguments unchanged. After providing the arguments correctly, and continuing with the program, we will see the flag in the serial monitor.

Read [Debugger Flag — 2025 eCTF 2025.01.15 documentation](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html) for details

> _**NOTE:** Add Microsoft's [Serial Monitor](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-serial-monitor "Send and receive text from serial ports.") extension in Visual Studio Code to monitor the serial output.
> Open **Serial Monitor** in VS Code by pressing `Ctrl+Shift+P` and typing `View: Toggle Serial Monitor`. Select the **port** and click **Start Monitor** button._

Here are the steps (without explanation) to get the `debugger_flag`.

In GDB console, run to following commands one by one.

#### Get Value1
```sh
break gdb_challenge
continue
x to_hex  # Copy the output as value1 (Hint: starts with 0x4c)
```

#### Get Value2
```sh
break do_some_math
continue
info registers  # Look for 'sp' value2 (Hint: ends with 0xe0)
```

> Details on [Setting Breakpoint](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html#setting-a-breakpoint)

#### Get Value3

```sh
watch $r2
commands
info registers r2
end
continue  # Repeat this until $r2 starts with 0xe1 (Hint: continue 6  times)
delete <watchpoint_number>  # Example: delete 3
```

> Details on [Stepping Through Code](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html#stepping-through-code) and [Setting a Watchpoint](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html#setting-a-watchpoint)

#### Capturing the Flag

We have the required 3 values, time to set the arguments.
```sh
break check_flag
continue
```

Inspect the register values before modifying them by `info registers`
Example:

```sh
(gdb) info registers
r0             0x11111111          286331153
r1             0x22222222          572662306
r2             0x33333333          858993459
r3             0x44444444          1145324612
r4             0x0                 0
r5             0x0                 0
r6             0x0                 0
r7             0x2001ffe8          537001960
r8             0x0                 0
r9             0x0                 0
r10            0x0                 0
r11            0x0                 0
r12            0xf4240000          -198967296
sp             0x2001ffe0          0x2001ffe0
lr             0x1000e60b          268494347
pc             0x1000e4b0          0x1000e4b0 <check_flag>
xPSR           0x61000000          1627389952
fpscr          0x0                 0
msp            0x2001ffe0          0x2001ffe0
psp            0x0                 0x0
primask        0x0                 0
basepri        0x0                 0
faultmask      0x0                 0
control        0x0                 0
(gdb) x $sp
0x2001ffe0:     0x55555555
```

According to the [Capturing the flag](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html#capturing-the-flag) instruction, we will update the `$r0`, `$r2`, and `$sp` values.

```sh
set $r0 = value1
set $r2 = value2
set *0x2001ffe0 = value3
```

Verify the updates:
```sh
info registers r0
info registers r2
x $sp
```

If everything went right, open a **Serial Monitor** and connect to the device's port. In the GDB console, `continue` the program and watch the Serial Monitor for the flag.

> _**ALERT:** If you see `Info : [max32xxx.cpu] external reset detected` in OpenOCD CLI, restart the [GDB console](#running-gdb-in-docker) and repeat [this step](#capturing-the-flag)._

> Details on [Writing to Registers and Memory](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html#writing-to-registers-and-memory) and [Capturing the flag](https://ectfmitre.gitlab.io/ectf-website/2025/flags/debugger_flag.html#capturing-the-flag)

---

###### 0x4c09b410, 0x2001ffe0, 0xe13f7732
