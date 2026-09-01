# BC3Miner

High performance SHA3-256T GPU mining for BitcoinIII (BC3).

Native NVIDIA CUDA and AMD OpenCL mining for Windows, Linux, and HiveOS.

![Version 1.0.3](https://img.shields.io/badge/version-1.0.3-168f77) ![Windows 10 and 11](https://img.shields.io/badge/Windows-10%20%7C%2011-3578e5) ![Linux and HiveOS](https://img.shields.io/badge/Linux-HiveOS-5f6f52) ![NVIDIA CUDA](https://img.shields.io/badge/NVIDIA-CUDA-76b900)

![AMD OpenCL](https://img.shields.io/badge/AMD-OpenCL-d94841) ![Optional 0.50 percent developer fee](https://img.shields.io/badge/dev%20fee-optional%200.50%25-168f77) ![MIT License](https://img.shields.io/badge/license-MIT-555555)

[Download](#downloads) · [Quick Start](#quick-start) · [Performance](#performance) · [Supported GPUs](#supported-gpus) · [Developer Fee](#optional-developer-fee) · [Security](#security-and-privacy) · [Support](#reporting-issues)

## Overview

BC3Miner mines BitcoinIII (BC3) using SHA3-256T. The Windows package includes a native GUI and CLI for NVIDIA CUDA, the Linux/HiveOS package provides a CLI for NVIDIA CUDA and AMD OpenCL. Architecture paths are selected for supported hardware below with approved compatibility behavior where available.

You choose the pool, wallet or user, worker and pool password or difficulty setting. Every GPU candidate is revalidated on the CPU before submission. There is an optional developer fee that is enabled by default and can be completely disabled in the saved configuration if you wish.

## Highlights

| Capability | What it provides |
| --- | --- |
| **Performance** | Optimized for NVIDIA and tuned for AMD OpenCL. |
| **Cross platform** | Windows GUI/CLI plus Linux and HiveOS CLI packages. |
| **Backends** | NVIDIA CUDA and AMD OpenCL. |
| **Live mining** | Compatible BC3 Stratum pools with separate connection, job, and submission accounting. |
| **Share safety** | CPU candidate revalidation, stale job handling, duplicate suppression, nonce accounting, and exact difficulty ledgers. |
| **Transparent fee** | Optional 0.50% developer fee, enabled by default and disabled in the config file. |
| **Privacy** | No hidden telemetry, remote control service, autostart, or automatic updater. |
| **Hardware policy** | No Auto Tune and no miner controlled power, clock, voltage, fan, VBIOS, or firmware changes. |

## Downloads

Download BC3Miner only from the [official GitHub release](https://github.com/evilwhynot/bc3-sha3-256t-miner/releases/tag/v1.0.3).

| Platform | Package | Includes |
| --- | --- | --- |
| Windows 10/11 x86_64 | [Windows ZIP](https://github.com/evilwhynot/bc3-sha3-256t-miner/releases/download/v1.0.3/bc3-miner-v1.0.3-windows-x86_64-nvidia-cuda-gui-cli.zip) | Native GUI, CLI, NVIDIA CUDA |
| Linux / HiveOS x86_64 | [Linux tar.gz](https://github.com/evilwhynot/bc3-sha3-256t-miner/releases/download/v1.0.3/bc3miner-v1.0.3.tar.gz) | CLI, NVIDIA CUDA, AMD OpenCL |
| Verification | [SHA256SUMS](https://github.com/evilwhynot/bc3-sha3-256t-miner/releases/download/v1.0.3/SHA256SUMS-V1_0_3.txt) | Package integrity checks |

## Quick Start

### Windows GUI

1. Download and extract the Windows package.
2. Open `bc3-miner-gui.exe`.
3. Enter your pool, wallet or user, worker, and password or difficulty.
4. Select the GPU and mining mode.
5. Click **Start Mining**.
6. Monitor hashrate and shares in **Activity**.
7. Click **Stop Mining** to end the local miner process.

The GUI does not start mining automatically. One Start action creates one miner child, there is no secondary confirmation popup, and Auto Tune is not included.

### Windows CLI

```powershell
.\bc3-miner.exe mine `
  --pool "YOUR_POOL" `
  --wallet "YOUR_BC3_ADDRESS" `
  --worker "YOUR_WORKER" `
  --password "YOUR_PASSWORD" `
  --backend cuda `
  --device 0 `
  --enable-live-mining
```

### Linux CLI

```bash
./bc3-miner mine \
  --pool "YOUR_POOL" \
  --wallet "YOUR_BC3_ADDRESS" \
  --worker "YOUR_WORKER" \
  --password "YOUR_PASSWORD" \
  --backend cuda \
  --device 0 \
  --enable-live-mining
```

For a HiveOS flight sheet, continue to [HiveOS Setup](#hiveos-setup).

## Supported Platforms

| Platform | Backend | Interface | Requirements |
| --- | --- | --- | --- |
| Windows 10/11 x86_64 | NVIDIA CUDA | Native GUI and CLI | Compatible NVIDIA driver/runtime |
| Linux x86_64 | NVIDIA CUDA, AMD OpenCL | CLI | Ubuntu 22.04 class environment, OpenSSL 3 |
| Current HiveOS x86_64 | NVIDIA CUDA, AMD OpenCL | Custom miner CLI | Current supported HiveOS generation, OpenSSL 3 |

Driver and runtime compatibility can affect whether a precompiled CUDA image loads. Approved automatic fallback behavior may be used where available; strict precompiled verification fails visibly rather than silently selecting a different image.

> **AMD compatibility note:** AMD/OpenCL mining retains its previously validated mining and performance implementation. The optional v1.0.3 developer-fee dual-session runtime was not physically revalidated on AMD because the authorized AMD test hosts were unavailable. Session switching remains covered by software, mock-Stratum, and multi-device accounting tests.

## Supported GPUs

### NVIDIA Models Supported

- GeForce RTX 50XX
- GeForce RTX 40XX
- GeForce RTX 30XX
- GeForce RTX 20XX
- GeForce GTX 10XX

### AMD OpenCL

- Radeon RX 5700 XT (`gfx1010`) has a validated performance path.
- Other AMD GPUs may work through OpenCL but they are not described as optimized or validated.

## Performance

[Hashrate of tested GPUs](https://docs.google.com/spreadsheets/d/12XgP8UFg0mFIDvssOLfgTR3HHrnOwDVcYN4OYCC1xkA/edit?gid=987654321#gid=987654321)

### NVIDIA

```text
RTX 5090: v1.0.3: 2,528.92 MH/s
RTX 4090: v1.0.3: 1,794.38 MH/s
RTX 5070: v1.0.3: 724.68 MH/s
RTX 3090: v1.0.3: 771.59 MH/s
```

### AMD results

```text
RX 5700 XT: v1.0.3: HiveOS: 245.54 MH/s
```

Results are per GPU and reflect specific tested systems. Performance varies with GPU model, driver, clocks, power, cooling, operating system, pool, and difficulty. **Wall rate** is the practical local mining rate; **kernel rate** isolates GPU execution.

## Optional Developer Fee

**BC3Miner includes an optional 0.50% developer mining fee.**

It is enabled by default and can be completely disabled in the saved configuration:

```toml
developer_fee_enabled = false
```

Accounting uses exact completed physical hashes with a target ratio of 199 user parts to 1 developer part. User and developer work use separate Stratum sessions and separate submission ledgers. Developer work connects to the disclosed HashBay endpoint below. If the developer session is unavailable, user mining continues. Dry run, benchmark, verification, and self test modes perform no developer fee work; disabled mode creates no developer connection.

| Policy field | Value |
| --- | --- |
| Endpoint | `stratum+tcp://stratum.hashbay.io:3344` |
| Public payout wallet | `15zH2Lfcri1nGeNLLyrA1E9Z431rEtwkKK` |
| Default | Enabled |
| Complete opt out | `developer_fee_enabled = false` |

## Security and Privacy

- Every GPU candidate is CPU revalidated before submission.
- Stale and duplicate candidates are suppressed, and nonce ranges are accounted for.
- User and developer sessions keep independent submission and difficulty ledgers.
- Wallet passwords, private keys, and seed phrases are not required or stored.
- BC3Miner v1.0.3 does not include Auto Tune or controls for GPU power, clocks, voltage, fans, VBIOS, or firmware.
- Support bundles remain local and should be reviewed before sharing.

### Windows antivirus and signing

The exact final v1.0.3 package passed the local Microsoft Defender normal-protection scan with no new detections. This does not establish universal antivirus approval or SmartScreen reputation.

Cryptocurrency mining software may be flagged by antivirus or endpoint security products. The v1.0.3 Windows executables are unsigned and may display a Microsoft SmartScreen warning.

## Verify Downloads

| File | SHA256 |
| --- | --- |
| `bc3-miner-v1.0.3-windows-x86_64-nvidia-cuda-gui-cli.zip` | `2bb5988021cf1ca4041989b8300e8aa614406b8ffc5672b12d204a7b7c632296` |
| `bc3miner-v1.0.3.tar.gz` | `128b4df0c309c32287ae65186f268d6c12e3892e0af8fd958838abfbd4147405` |
| `SHA256SUMS-V1_0_3.txt` | `8a944355e9ca1057b46d6a68e957653007a67cf1cab0fa638c74a0fe9c4300fd` |

Windows PowerShell:

```powershell
Get-FileHash .\bc3-miner-v1.0.3-windows-x86_64-nvidia-cuda-gui-cli.zip -Algorithm SHA256
```

Linux, with both package archives and the checksum file in the same directory:

```bash
sha256sum -c SHA256SUMS-V1_0_3.txt
```

The reported hashes must match the official `SHA256SUMS-V1_0_3.txt` file and the values above.

## Configuration

BC3Miner accepts a TOML configuration file. Start from the packaged example and replace every placeholder:

```toml
pool = "YOUR_POOL"
wallet = "YOUR_BC3_ADDRESS"
worker = "YOUR_WORKER"
password = "YOUR_PASSWORD"
backend = "cuda"
device = "0"
developer_fee_enabled = true
```

To disable the optional developer fee completely:

```toml
developer_fee_enabled = false
```

Run the CLI with the saved configuration:

```powershell
.\bc3-miner.exe mine --config .\bc3-miner.toml --enable-live-mining
```

Use `backend = "opencl"` on supported AMD/Linux configurations. Advanced geometry and runtime options should be changed only when their effect is understood.

## HiveOS Setup

Create a custom miner in HiveOS with these fields:

```text
Miner name: bc3miner
Installation URL: https://github.com/evilwhynot/bc3-sha3-256t-miner/releases/download/v1.0.3/bc3miner-v1.0.3.tar.gz
Hash algorithm: sha-3
Pool URL: YOUR_POOL
Password: YOUR_PASSWORD
```

Use your own wallet and worker template. In the extra configuration, select one backend:

```text
BC3_BACKEND=cuda
```

```text
BC3_BACKEND=opencl
```

To disable the optional developer fee through the HiveOS wrapper:

```text
BC3_DEV_FEE_ENABLED=0
```

This writes the authoritative TOML setting `developer_fee_enabled = false`.
HiveOS remains responsible for clocks, power, voltage, and fan settings. The
package also includes the complete HiveOS setup guide.

## Troubleshooting / FAQ

<details>
<summary><strong>Does BC3Miner charge a developer fee?</strong></summary>

BC3Miner includes an optional 0.50% developer fee. It is enabled by default, uses a separate HashBay Stratum session and is fully disclosed above.
</details>

<details>
<summary><strong>How do I disable the developer fee?</strong></summary>

Set `developer_fee_enabled = false` in the saved TOML configuration. On HiveOS, set `BC3_DEV_FEE_ENABLED=0`.
</details>

<details>
<summary><strong>Can I use a pool other than HashBay?</strong></summary>

Yes. User mining can use a compatible BC3 Stratum pool. When enabled, only the optional developer fee session connects to HashBay.
</details>

<details>
<summary><strong>Does BC3Miner overclock or change my GPU settings?</strong></summary>

No. BC3Miner v1.0.3 does not include Auto Tune and does not change power, clocks, voltage, fans, VBIOS, or firmware.
</details>

<details>
<summary><strong>Why might Windows display an antivirus or SmartScreen warning?</strong></summary>

Mining software can trigger security heuristics and the portable Windows executables are unsigned. Download only from the official release and verify the SHA256 checksum. Do not disable security controls.
</details>

<details>
<summary><strong>What information should I include in a bug report?</strong></summary>

Include the BC3Miner version, operating system, GPU model and count, driver, CUDA or OpenCL runtime, backend, wall and kernel rates, accepted/rejected/stale counts, sanitized errors, and whether the issue reproduces in dry run mode.
</details>

## Reporting Issues

Use [GitHub Issues](https://github.com/evilwhynot/bc3-sha3-256t-miner/issues) for reproducible bugs. Search existing reports first and include enough system detail to reproduce the problem.

Never post private keys, seed phrases, wallet passwords, exchange credentials, SSH credentials, or raw configuration files containing secrets. Sanitize pool credentials, worker names, IP addresses, and local paths before sharing logs or support bundles.

## License

BC3Miner is distributed under the MIT License included with each package. The
presence of that license does not itself describe the repository's
source publication policy.

## Disclaimer

Cryptocurrency mining consumes electricity and can place sustained load on hardware. Results vary. You are responsible for hardware limits, electricity costs, pool selection, taxes, and compliance with local laws. No profitability or hashrate guarantee is made.
