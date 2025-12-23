# About

Welcome to NodeTunnel’s documentation website.

NodeTunnel is an open-source multiplayer peer for **Godot**, created by **curtjs**, focused on making “host/join” multiplayer feel simple even when players are behind NATs or restrictive networks.

## Why NodeTunnel exists

Godot’s high-level multiplayer is great once players can connect — the hard part is *getting* that connection working for real players without:
- NAT traversal headaches
- port forwarding
- dedicated servers just to get started

NodeTunnel’s goal is to remove those setup barriers by routing traffic through a relay server so peers can communicate regardless of network setup.

## How it works

NodeTunnel’s basic flow looks like this:
1. Connect to a relay server
2. Receive an **Online ID** (example: `CRRJS`)
3. One player hosts, the other joins using that ID
4. From there, you use **normal Godot multiplayer** (RPCs, authority, spawners, etc.)

In other words: the relay forwards packets between players so they can connect even when they can’t directly reach each other.

## Project status

NodeTunnel is currently **early / experimental**. Expect bugs, breaking changes, and API changes. It’s not recommended for heavy production use yet.

## Free public relay

curtjs provides a **free public relay** for testing:

- Host: `relay.nodetunnel.io`
- Port: `9998`

Don’t rely on it for anything important — uptime is not guaranteed, and self-hosting resources are still evolving.

## License and community

NodeTunnel is released under the **MIT License**.

For help, bug reports, and updates, use:
- NodeTunnel's download page - https://www.nodetunnel.io/
- GitHub Issues
- the project’s Discord server 
- for video tutorials visit curtjs's amazing youtube channel at - https://www.youtube.com/@curtjs-dev

---

To keep nodetunnel free for everyone, please consider donating at - https://ko-fi.com/curtjs