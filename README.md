# BC3 Miner

High performance BC3Hash GPU mining for BitcoinIII (BC3).

NVIDIA CUDA and AMD Radeon mining for Windows, Linux, and HiveOS.

![Version 2.1.3](https://img.shields.io/badge/version-2.1.3-168f77) ![Windows 10 and 11](https://img.shields.io/badge/Windows-10%20%7C%2011-3578e5) ![Linux and HiveOS](https://img.shields.io/badge/Linux-HiveOS-5f6f52) ![NVIDIA CUDA](https://img.shields.io/badge/NVIDIA-CUDA-76b900)

![AMD Radeon](https://img.shields.io/badge/AMD-OpenCL%20%7C%20ROCm-d94841) ![1 percent developer fee](https://img.shields.io/badge/dev%20fee-1%25%2C%20can%20be%20turned%20off-168f77) ![MIT License](https://img.shields.io/badge/license-MIT-555555)

[Download](#downloads) · [Quick Start](#quick-start) · [Performance](#performance) · [Supported GPUs](#supported-gpus) · [Developer Fee](#developer-fee) · [Security](#security-and-privacy) · [Support](#reporting-issues)

## Overview

BC3 Miner mines BitcoinIII (BC3) with BC3Hash, the proof of work BitcoinIII has used since block 64360. The Windows package has a desktop app and a command line miner for NVIDIA and AMD GPUs. The Linux package is a command line miner for NVIDIA and AMD GPUs, and the HiveOS package is a custom miner built from it.

You choose the pool, wallet, worker and password. Every result a GPU finds is hashed again on the CPU with the BitcoinIII node's own BC3Hash code before it is sent to the pool. A 1% developer fee is on by default and can be turned off.

> **Upgrading from v1.0.x:** BitcoinIII replaced SHA3-256T with BC3Hash at block 64360, so BC3Miner v1.0.x no longer mines BC3. Install v2.1.3. On HiveOS, change the flight sheet's Installation URL (see [HiveOS Setup](#hiveos-setup)).

> **Windows users of v2.0.0 to v2.1.2:** update to v2.1.3. Those versions crash at the start of mining whenever Windows has been running for less than an hour, for example after a restart (see [Troubleshooting / FAQ](#troubleshooting--faq)).

## Highlights

| Capability | What it provides |
| --- | --- |
| **BC3Hash** | Mining of BitcoinIII's current proof of work on NVIDIA and AMD GPUs. |
| **Share safety** | Every result is hashed again on the CPU with the BitcoinIII node's BC3Hash code. The GPUs also report easier results that are only checked, so their correctness is confirmed all the time. |
| **Desktop app** | Windows app for HashBay pools (North America or Europe, SOLO or PROP) or any stratum+tcp BC3Hash pool, with a GPU self test, temperatures and session logs. |
| **AMD Radeon (beta)** | Windows through the Radeon driver's OpenCL. Linux and HiveOS with the AMD runtime included, so no ROCm installation is needed. RX 7000 also under WSL 2. |
| **Stuck GPU watchdog** | A GPU that stops responding makes the miner restart itself. A GPU that keeps failing is left out, so the other GPUs keep mining. |
| **Temperature limits** | A GPU that reaches its core or memory temperature limit pauses until it has cooled down. |
| **Efficiency** | The hashrate for each watt of GPU power (kH/W), next to each GPU's power in the desktop app, on the miner's speed line and in the statistics file. |
| **Driver check** | GPUs that need a newer NVIDIA driver are named before mining starts. |
| **Error reports** | `bc3-miner report` writes one file for a bug report, with your wallet address, worker name and pool password removed. |
| **Hardware policy** | No miner controlled power, clock, voltage or fan changes. |
| **Privacy** | Connects only to your pool and, while the developer fee is on, to HashBay. Nothing is uploaded. |

## Downloads

Download BC3 Miner only from the [official GitHub release](https://github.com/evilwhynot/BC3-Miner/releases/tag/v2.1.3).

| Platform | Package | Includes |
| --- | --- | --- |
| Windows 10/11 x86_64 | [Windows ZIP](https://github.com/evilwhynot/BC3-Miner/releases/download/v2.1.3/bc3-miner-v2.1.3-windows-x86_64-nvidia-amd-gui-cli.zip) | Desktop app, command line miner, NVIDIA CUDA, AMD OpenCL |
| Linux x86_64 | [Linux tar.gz](https://github.com/evilwhynot/BC3-Miner/releases/download/v2.1.3/bc3-miner-v2.1.3-linux-x86_64-nvidia-amd.tar.gz) | Command line miner, NVIDIA CUDA, AMD runtime |
| HiveOS x86_64 | [HiveOS tar.gz](https://github.com/evilwhynot/BC3-Miner/releases/download/v2.1.3/bc3miner-2.1.3.tar.gz) | Custom miner `bc3miner` |
| LGPL sources | [Sources tar.gz](https://github.com/evilwhynot/BC3-Miner/releases/download/v2.1.3/bc3-miner-v2.1.3-linux-lgpl-sources.tar.gz) | Source of the libelf and libnuma libraries in the Linux and HiveOS packages |
| Verification | [SHA256SUMS](https://github.com/evilwhynot/BC3-Miner/releases/download/v2.1.3/SHA256SUMS-V2_1_3.txt), plus an `.asc` signature for each package | Package integrity and signature checks |

## Quick Start

### Windows desktop app

1. Download and extract the Windows package.
2. Run `bc3-miner-gui.exe`. It finds your NVIDIA and AMD GPUs by itself.
3. On the **Mine** page, choose a pool (HashBay SOLO or PROP, or **Other pool**) and, for HashBay, the region nearest to you. Enter your BC3 wallet address and a worker name.
4. Optional: click **Test GPUs** once. Each GPU's results are checked against the CPU.
5. Click **Start mining**. The page shows each step while the GPUs get ready, then the hashrate, shares and temperatures.
6. Click **Stop mining** to end the session.

Settings are saved automatically. Intensity, curve table size, temperature limits, dry run and a time limit are under **Advanced options** on the Mine page and on the Settings page.

### Windows command line

```powershell
.\bc3-miner.exe mine `
  --pool stratum+tcp://na.stratum.hashbay.io:3345 `
  --wallet YOUR_BC3_ADDRESS `
  --worker YOUR_WORKER
```

### Linux

```bash
tar -xzf bc3-miner-v2.1.3-linux-x86_64-nvidia-amd.tar.gz
cd bc3-miner-v2.1.3-linux-x86_64-nvidia-amd
./bc3-miner gpu-list
./bc3-miner verify
./bc3-miner mine \
  --pool stratum+tcp://na.stratum.hashbay.io:3345 \
  --wallet YOUR_BC3_ADDRESS \
  --worker YOUR_WORKER
```

HashBay servers: `na.stratum.hashbay.io` (North America) and `eu.stratum.hashbay.io` (Europe), each with SOLO on port 3345 and PROP on port 3344.

For a HiveOS flight sheet, continue to [HiveOS Setup](#hiveos-setup).

## Supported Platforms

| Platform | GPUs | Interface | Requirements |
| --- | --- | --- | --- |
| Windows 10/11 x86_64 | NVIDIA, AMD (beta) | Desktop app and command line | NVIDIA driver 580 or newer, or a current AMD Radeon (Adrenalin) driver |
| Linux x86_64 | NVIDIA, AMD | Command line | glibc 2.17 or newer. NVIDIA driver 580 or newer. AMD: the amdgpu driver with `/dev/kfd` and glibc 2.31 or newer |
| HiveOS x86_64 | NVIDIA, AMD | Custom miner | NVIDIA driver 580 or newer. AMD: the HiveOS amdgpu driver |
| Windows 11 with WSL 2 | AMD RX 7000 | Linux command line | Adrenalin 26.2.2 or newer, Ubuntu 22.04 or 24.04 |

On Linux, run the miner as root or as a user in the `render` and `video` groups to use AMD GPUs.

## Supported GPUs

### NVIDIA Models Supported

- GeForce RTX 50XX
- GeForce RTX 40XX
- GeForce RTX 30XX
- GeForce RTX 20XX and GTX 16XX (supported by the build, not tested yet)
- GeForce GTX 10XX (needs the 580 driver series)
- The matching workstation cards

Not supported: GTX 9XX (Maxwell), Volta (Titan V, Tesla V100), and data-center GPUs such as A100, H100 and B200.

### AMD Radeon (beta)

- **Windows:** RX 400 series and newer (RX 400/500, Vega, RX 5000, 6000, 7000 and 9000), through the OpenCL runtime of the Radeon (Adrenalin) driver. Not yet tested on Windows Radeon drivers. Integrated Radeon graphics are not used.
- **Linux and HiveOS:** RX 5000 (RDNA 1), RX 6000 (RDNA 2) and RX 7000 (RDNA 3) and the matching Radeon Pro cards. Tested on RX 5700 XT (HiveOS) and RX 7900 XTX (WSL 2). RX 400/500, Vega, Radeon VII and RX 9000 are not supported.
- **Windows 11 through WSL 2:** RX 7000 series, with the Linux package.

### GPU memory

The miner builds a curve table on each GPU when it starts and picks the largest (fastest) table that fits: 16.1 GB on 24 GB NVIDIA cards, 4.6 GB on 8 to 16 GB cards and 1.5 GB on 6 GB cards. AMD GPUs use 21.5 GB on 24 GB cards and 5.9 GB on 8 to 16 GB cards.

## Performance

### NVIDIA

```text
RTX 5090:       v2.1.0: 5.00-5.07 MH/s  (Linux, 575 W)
RTX 4090 48GB:  v2.1.0: 2.85-2.87 MH/s  (Linux, 450 W)
RTX 3090:       v2.1.2: 1.76-1.77 MH/s  (Windows, 449 W)            4.0 kH/W
RTX 3080 10GB:  v2.1.2: 1.44 MH/s       (HiveOS rig, 12-row table)  4.3 kH/W
RTX 3060 Ti:    v2.1.2: 0.86-0.88 MH/s  (HiveOS rig, 12-row table)  3.3-4.0 kH/W
GTX 1070 Ti:    v2.1.2: 0.13 MH/s       (HiveOS rig, 12-row table)  0.85 kH/W
```

### AMD results

```text
RX 5700 XT x5:  v2.1.2: HiveOS: 1.56 MH/s total (0.31 MH/s each), 2.3 kH/W (GPU chip power)
RX 7900 XTX:    v2.1.0: WSL 2: 0.93 MH/s (45 second runs)
```

60 second benchmarks at default power limits, except the RX 5700 XT rig, which runs its own clock and power settings. kH/W is the efficiency while mining on HashBay: the hashrate for each watt the GPU driver reports (the whole card on NVIDIA, the GPU chip alone on AMD). Results reflect specific tested systems. Performance varies with GPU model, driver, clocks, power, cooling, operating system and pool.

## Developer Fee

**BC3 Miner includes a 1% developer fee.** It is on by default and can be turned off.

Once every 100 minutes, the GPUs mine for 60 seconds (1% of the time) for the BC3 Miner developer on HashBay PROP, and for you the rest of the time. The first window comes at a random time in the first 100 minutes. The log and the app's Activity page show each window, and its shares are counted separately from yours, so your pool sees about 1% less hashrate than the miner shows. If the developer pool cannot be reached, the window is skipped and mining goes on for you.

| Policy field | Value |
| --- | --- |
| Endpoint | `stratum+tcp://na.stratum.hashbay.io:3344` (`eu.stratum.hashbay.io:3344` when you mine on HashBay Europe) |
| Developer worker | `15zH2Lfcri1nGeNLLyrA1E9Z431rEtwkKK.devfee` |
| Default | On |
| Turn off | Desktop app: **Settings > Developer fee**. Command line: `--no-dev-fee`. HiveOS: `--no-dev-fee` in the flight sheet's Extra config arguments |

With the fee turned off, the miner never connects to the developer's pool.

## Security and Privacy

- Every share is hashed again on the CPU with the BitcoinIII node's BC3Hash code before it is submitted. If a GPU result ever differs, it is dropped and that GPU is reset.
- BC3 Miner does not change GPU power, clocks, voltage or fans.
- The miner connects only to the pool you choose and, while the developer fee is on, to HashBay for the fee windows. Nothing is uploaded.
- The desktop app saves its settings, including your wallet address and pool password, in `%APPDATA%\BC3Miner\gui-settings.toml`. **Settings > Wallet and privacy** turns this off or removes the saved wallet.
- Logs and the Activity page hide the wallet address and pool password. Error reports remove the wallet address, worker name and pool password.
- Wallet passwords, private keys and seed phrases are never needed.

## Verify Downloads

Every package is signed with the HashBay.io Software OpenPGP key (on keys.openpgp.org):

```text
E0F6 CCD1 5264 35B4 1773  E4F1 E031 BF02 8477 0E9E
```

| File | SHA256 |
| --- | --- |
| `bc3-miner-v2.1.3-windows-x86_64-nvidia-amd-gui-cli.zip` | `e0f9ce50efa2439c970c99c6d11331385f2df943bc09f04c73296c5c95490698` |
| `bc3-miner-v2.1.3-linux-x86_64-nvidia-amd.tar.gz` | `8968b5a0eb958e86a8062358ba53de31b386d62e757b97fc1a736c71ba7695ae` |
| `bc3miner-2.1.3.tar.gz` | `1b7c2e2937a53a4583daa279266a36d0bba6d838d2b92b8333ab5165fecd8bd5` |
| `bc3-miner-v2.1.3-linux-lgpl-sources.tar.gz` | `0b8e6013af62336c6d36d470d39a90e2f8d70ed870304f451090e415c6506799` |
| `SHA256SUMS-V2_1_3.txt` | `bbd8f495039de8150ac7beb63f1fe1915628442400c68ef5d20aed40b95af23d` |

Check the signature (Linux, or Windows with Gpg4win):

```bash
gpg --verify bc3-miner-v2.1.3-windows-x86_64-nvidia-amd-gui-cli.zip.asc bc3-miner-v2.1.3-windows-x86_64-nvidia-amd-gui-cli.zip
```

Windows PowerShell:

```powershell
Get-FileHash .\bc3-miner-v2.1.3-windows-x86_64-nvidia-amd-gui-cli.zip -Algorithm SHA256
```

Linux, with the checksum file in the same directory as the packages:

```bash
sha256sum -c --ignore-missing SHA256SUMS-V2_1_3.txt
```

Each package also contains `SHA256SUMS.txt` and its signature `SHA256SUMS.txt.asc` for the files inside it.

## Command Line Options

```text
--pool URL                         stratum+tcp:// pool address
--wallet ADDRESS                   BC3 wallet address (or pool username)
--worker NAME                      worker name
--password X                       pool password or difficulty setting (default x)
--device all | 0,1                 GPUs to use (numbers from gpu-list)
--backend auto|nvidia|amd          GPU vendors to use (default: all supported GPUs)
--intensity eco|balanced|high|max  batch size per GPU (default max)
--curve-rows auto|11|12|13|14|16   curve table size (default: the largest that fits)
--temp-limit 90                    pause a GPU at this core temperature (C)
--mem-temp-limit 105               pause a GPU at this memory temperature (C)
--dry-run                          mine and check shares but never submit them
--no-dev-fee                       turn off the developer fee
```

`bc3-miner mine --help` lists every option. Other commands: `gpu-list`, `verify`, `benchmark --seconds 60`, `sensors` and `report`.

## HiveOS Setup

Create a flight sheet with a custom miner:

```text
Coin:                        BC3 (add it as a custom coin if it is not listed)
Wallet:                      your BC3 address
Pool:                        Configure in miner
Miner:                       Custom
Miner name:                  bc3miner
Installation URL:            https://github.com/evilwhynot/BC3-Miner/releases/download/v2.1.3/bc3miner-2.1.3.tar.gz
Hash algorithm:              bc3hash
Wallet and worker template:  %WAL%.%WORKER_NAME%
Pool URL:                    stratum+tcp://na.stratum.hashbay.io:3345
Pass:                        x
Extra config arguments:      optional, for example --intensity high --temp-limit 85
```

The same flight sheet works on NVIDIA, AMD and mixed rigs. HiveOS downloads a custom miner again only when the Installation URL changes, so point the flight sheet at the new file to update. Add `--no-dev-fee` to the Extra config arguments to turn the developer fee off.

NVIDIA rigs need driver 580 or newer (`nvidia-driver-update 580`); rigs with GTX 10XX cards need the 580 series. HiveOS remains responsible for clocks, power, voltage and fan settings. The package's README.txt has the complete HiveOS guide.

## Troubleshooting / FAQ

<details>
<summary><strong>The miner crashes right after it starts on Windows ("overflow when subtracting duration from instant").</strong></summary>

Update to v2.1.3. Versions 2.0.0 to 2.1.2 crash this way at the start of mining whenever Windows has been running for less than an hour, for example after a restart or a Windows update, and the miner keeps restarting itself until the hour has passed. Linux and HiveOS are not affected.
</details>

<details>
<summary><strong>Does BC3 Miner charge a developer fee?</strong></summary>

Yes, 1%, on by default: 60 seconds in every 100 minutes on HashBay PROP. It is fully disclosed above.
</details>

<details>
<summary><strong>How do I turn off the developer fee?</strong></summary>

In the desktop app, use **Settings > Developer fee**. On the command line and on HiveOS, add `--no-dev-fee`.
</details>

<details>
<summary><strong>Can I use a pool other than HashBay?</strong></summary>

Yes. Any stratum+tcp BC3Hash pool works (**Other pool** in the desktop app). While the developer fee is on, only its windows connect to HashBay.
</details>

<details>
<summary><strong>Does BC3 Miner overclock or change my GPU settings?</strong></summary>

No. BC3 Miner does not change power, clocks, voltage or fans.
</details>

<details>
<summary><strong>What does the efficiency figure (kH/W) mean?</strong></summary>

It is the hashrate for each watt of GPU power over the last 10 seconds: 1.78 MH/s at 440 W is 4.05 kH/W, the same as 4.05 kH per joule. The power is what the GPU driver reports: the whole card on NVIDIA, the GPU chip alone on AMD, so AMD figures look better, and the PC draws more at the wall. A GPU shows it once it has hashed for 12 seconds, and not while it is paused at its temperature limit.
</details>

<details>
<summary><strong>Does BC3Miner v1.0.x still work?</strong></summary>

No. BitcoinIII switched from SHA3-256T to BC3Hash at block 64360, and v1.0.x mined SHA3-256T. Install v2.1.3. On HiveOS, change the flight sheet's Installation URL.
</details>

<details>
<summary><strong>The miner says "not enough free GPU memory".</strong></summary>

Close other programs that use the GPU, or choose a smaller curve table with `--curve-rows 12`, `13`, `14` or `16`.
</details>

<details>
<summary><strong>Why does the first start on an AMD GPU take longer on Windows?</strong></summary>

The Radeon driver compiles the miner's GPU code for your GPU model once, which takes from about 20 seconds to a few minutes. The result is kept and reused. After a driver update it is compiled once more.
</details>

<details>
<summary><strong>What should I include in a bug report?</strong></summary>

Run `bc3-miner report` (`bc3-miner.exe report` on Windows) and attach the file it writes. It holds the error reports and system details, with your wallet address, worker name and pool password removed. Describe what you did and what happened.
</details>

## Reporting Issues

Use [GitHub Issues](https://github.com/evilwhynot/BC3-Miner/issues) for reproducible bugs. Search existing reports first, and attach the file from `bc3-miner report`.

Never post private keys, seed phrases, wallet passwords, exchange credentials, SSH credentials, or raw configuration files containing secrets.

## License

BC3 Miner is distributed under the MIT License included with each package. The
presence of that license does not itself describe the repository's
source publication policy.

## Disclaimer

Cryptocurrency mining consumes electricity and can place sustained load on hardware. Results vary. You are responsible for hardware limits, electricity costs, pool selection, taxes, and compliance with local laws. No profitability or hashrate guarantee is made.
