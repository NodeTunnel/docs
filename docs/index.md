# About

Welcome to NodeTunnel’s documentation website.

NodeTunnel is a free and open-source Godot plugin that focuses on making "host/join" multiplayer feel simple, even when players are behind NATs or restrictive networks.

NodeTunnel does all this while still being compatible with Godot's high-level multiplayer API. **Switching only takes ~15 lines of code, regardless of how large your project is.**

## Why NodeTunnel exists

Godot’s high-level multiplayer is great once players can connect — the hard part is _getting_ that connection working for real players without:

- NAT traversal headaches
- Port forwarding
- Expensive dedicated servers

NodeTunnel’s goal is to remove those setup barriers by routing traffic through a relay server so peers can communicate regardless of network setup. **If you can see this website, NodeTunnel will work for you!**

## How it works

NodeTunnel’s basic flow looks like this:

1. Connect to a relay server
2. Host a room and receive a room ID (example: CRRJS)
3. One player hosts, the other joins using the room ID
4. From there, you use **normal Godot multiplayer** (RPCs, authority, spawners, etc.)

In other words: the relay forwards packets between players so they can connect even when they can’t directly reach each other.

## Who it's for

NodeTunnel isn't for everyone. There are some drawbacks to a relay architecture. NodeTunnel is best suited for smaller, co-operative/party games. Competitive shooters, MMOs, etc. are not a good fit for NodeTunnel.

- No authoritative server, so cheaters are difficult to prevent.
- Since packets have to be routed through a relay server, latency will be affected.
- HTML/web builds are not currently supported. Android/iOS are not officially tested, however, some have been able to get them to work.

## Project status

NodeTunnel is currently in the **beta phase**. Development is moving pretty quick, and bug reports have been fairly minimal so far. Regardless, expect breaking API changes and potential bugs. NodeTunnel is not recommended for large-scale projects at this time.

## Free public relays

**curtjs**, the creator of NodeTunnel, has hosted 2 **free-to-use** relay servers.

**US East:**

- Host: `us_east.nodetunnel.io:8080`

**EU Central:**

- Host: `eu_central.nodetunnel.io:8080`

During the beta phase, client compatibility will change abruptly. Do not rely on these servers until NodeTunnel is in the release phase.

## License and community

NodeTunnel is released under the **MIT License**.

For downloads, help, bug reports, and more, please use the helpful links below:

- [NodeTunnel download](https://www.nodetunnel.io/)
- [GitHub issues](https://github.com/NodeTunnel/godot-plugin/issues)
- [Discord](https://discord.gg/yDHnKmw7AS)
- Video tutorials are available [on curtjs' channel](https://www.youtube.com/@curtjs-dev).

---

**To keep nodetunnel free for everyone, [please consider donating.](https://ko-fi.com/curtjs)** All contributions are greatly appreciated.
