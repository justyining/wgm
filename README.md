# wgm - Simplified WireGuard Peer Management

A lightweight CLI tool for managing WireGuard peers with interactive prompts.

## Features

- Interactive peer addition without manual config editing
- Automatic IP address allocation
- Automatic config backup
- Client config file generation with QR code
- Support for manual or auto key generation

## Installation

```bash
npm install -g inquirer qrcode ini
```

Or use directly with npx:

```bash
sudo npx wgm add
```

## Usage

```bash
wgm add          # Add a new peer interactively
wgm list         # List all peers
wgm rm <name>    # Remove a peer
```

## Example

```bash
$ sudo wgm add
? Peer name: macbook
? Key generation method:
  ❯ I provide the client public key (recommended)
    Auto-generate key pair
? Paste client publicKey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Assigned IP: 10.0.0.3/24
✅ Peer "macbook" added successfully
📄 Config saved: macbook.conf
📱 QR Code:
[QR code displayed]
```

## Requirements

- WireGuard installed (`wg` command available)
- Node.js >= 16
- Root privileges

## License

MIT
