# wgm

[![npm version](https://img.shields.io/npm/v/wgm.svg)](https://www.npmjs.com/package/wgm)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A lightweight CLI tool for managing WireGuard peers with interactive prompts.

## Features

- **Interactive peer management** - Add/remove peers without manual config editing
- **Automatic IP allocation** - Finds next available IP in your network range
- **Config backup** - Automatically backs up your WireGuard config before changes
- **Client config generation** - Prints client config to terminal with QR codes
- **Flexible key generation** - Support for both manual (secure) and auto-generated keys

## Installation

```bash
npm install -g wgm
```

Or use with npx (no installation):

```bash
npx wgm add
```

## Requirements

- WireGuard installed (`wg` and `wg-quick` commands available)
- Node.js >= 16
- Root privileges (auto-requested when needed for reading/writing `/etc/wireguard/`)

## Usage

### Add a new peer

```bash
wgm add
```

Interactive workflow:
1. Enter peer name (e.g., `laptop`, `phone`)
2. Choose key generation method:
   - **Manual** (recommended): Client generates keys locally, you paste the public key
   - **Auto**: Tool generates keys on server (less secure)
3. Configure AllowedIPs (default: entire VPN network for client-to-client communication)
4. Tool prints client config to terminal with QR code

### List all peers

```bash
wgm list
```

Shows online/offline status and assigned IPs.

### Remove a peer

```bash
wgm rm <name>
```

## Example

```bash
$ wgm add
? Peer name: macbook
? Key generation method: I provide the client public key (recommended)

On the client device, run:
  wg genkey | tee privatekey | wg pubkey
Send the PUBLIC KEY to the server admin.

? Paste client public key: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Assigned IP: 10.0.0.3/32
? AllowedIPs (traffic to route through VPN): 10.0.0.0/24
[OK] Peer "macbook" added successfully

========== Client Config ==========
[Interface]
PrivateKey = YOUR_PRIVATE_KEY_HERE
Address = 10.0.0.3/32

[Peer]
PublicKey = SERVER_PUBLIC_KEY_HERE
AllowedIPs = 10.0.0.0/24
Endpoint = 1.2.3.4:51820
PersistentKeepalive = 25
===================================

[QR] QR Code:
[QR code displayed here]
```

## Client Setup

1. Copy the printed client config from terminal and save to a `.conf` file on your device
2. If you used manual key mode, replace the placeholder:
   - Remove the `#` comment character
   - Add your actual private key (from `wg genkey` output)
3. Import into WireGuard app, or use command line:
   ```bash
   sudo wg-quick up ./macbook.conf
   ```

## How It Works

- Reads/writes `/etc/wireguard/<interface>.conf`
- Automatically detects available WireGuard interfaces
- Creates timestamped backups in the same directory as the config file
- Reloads WireGuard configuration after changes

## Command Line Options

```bash
wgm [command] [options]

Commands:
  add       Add a new peer
  list      List all peers with online status
  rm <name> Remove a peer by name or public key
  init       Show initialization hint

Options:
  -i <name>        Specify WireGuard interface (default: auto-detect)
  --config <path>   Use custom config file
  --dry-run         Test mode (requires --config)
```

## Security Notes

- **Manual key generation** (recommended): Private key never leaves the client device
- **Auto key generation**: Private key is generated on the server; use only in trusted environments
- Always verify peer public keys before adding

## License

MIT © [justyining](https://github.com/justyining)
