# wgm

[![npm version](https://img.shields.io/npm/v/wgm.svg)](https://www.npmjs.com/package/wgm)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A lightweight CLI tool for managing WireGuard peers with interactive prompts.

## Features

- **Interactive peer management** - Add/remove peers without manual config editing
- **Automatic IP allocation** - Finds next available IP in your network range
- **Config backup** - Automatically backs up your WireGuard config before changes
- **Client config generation** - Creates ready-to-use `.conf` files with QR codes
- **Flexible key generation** - Support for both manual (secure) and auto-generated keys

## Installation

```bash
npm install -g wgm
```

Or use with npx (no installation):

```bash
# If npx is not in root's PATH, use full path:
sudo $(which npx) wgm add
```

## Requirements

- WireGuard installed (`wg` and `wg-quick` commands available)
- Node.js >= 16
- Root privileges (for reading/writing `/etc/wireguard/`)

## Usage

### Add a new peer

```bash
# If installed globally
sudo $(which wgm) add

# Or use full path
sudo /usr/local/bin/wgm add
```

Interactive workflow:
1. Enter peer name (e.g., `laptop`, `phone`)
2. Choose key generation method:
   - **Manual** (recommended): Client generates keys locally, you paste the public key
   - **Auto**: Tool generates keys on server (less secure)
3. Configure AllowedIPs (default: server VPN IP only)
4. Tool generates `peername.conf` file and QR code

### List all peers

```bash
wgm list
```

Shows online/offline status and assigned IPs.

### Remove a peer

```bash
sudo $(which wgm) rm <name>
```

## Example

```bash
$ sudo $(which wgm) add
? Peer name: macbook
? Key generation method: I provide the client public key (recommended)

On the client device, run:
  wg genkey | tee privatekey | wg pubkey
Send the PUBLIC KEY to the server admin.

? Paste client public key: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Assigned IP: 10.0.0.3/24
? AllowedIPs (traffic to route through VPN): 10.0.0.1/32
[OK] Peer "macbook" added successfully
[FILE] Config saved: macbook.conf
[QR] QR Code:
[QR code displayed here]
```

## Client Setup

1. Copy the generated `.conf` file to your client device
2. Import into WireGuard app, or use command line:
   ```bash
   sudo wg-quick up ./macbook.conf
   ```

## How It Works

- Reads/writes `/etc/wireguard/<interface>.conf`
- Automatically detects available WireGuard interfaces
- Creates timestamped backups before modifications
- Reloads WireGuard configuration after changes

## Security Notes

- **Manual key generation** (recommended): Private key never leaves the client device
- **Auto key generation**: Private key is generated on the server; use only in trusted environments
- Always verify peer public keys before adding

## License

MIT © [justyining](https://github.com/justyining)
