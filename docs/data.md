- **starting_out** (object):
  - **title** (string): `Starting Out`
  - **description** (string): `How to start using NodeTunnel in a Godot project.`
  - **overview** (object):
    - **shows** (array):
```json
[
  "what NodeTunnel needs to exist",
  "what the setup looks like",
  "how connecting, hosting, and joining work"
]
```

    - **notes** (string): `Code snippets are small and only meant to show what you should expect to see.`
  - **sections** (array):
    - Item 1:
```json
{
  "id": "nodetunnel_installed",
  "title": "NodeTunnel Installed",
  "content": [
    "NodeTunnel must already be available in your project.",
    "You should be able to reference the class:"
  ],
  "code": {
    "language": "gdscript",
    "snippet": "var peer := NodeTunnelPeer.new()\n"
  },
  "warning": "If this line errors, NodeTunnel is not installed correctly."
}
```

    - Item 2:
```json
{
  "id": "creating_peer",
  "title": "Creating a NodeTunnel Peer",
  "content": [
    "NodeTunnel works by providing a custom multiplayer peer.",
    "A peer is created like this:"
  ],
  "code": {
    "language": "gdscript",
    "snippet": "var peer := NodeTunnelPeer.new()\n"
  },
  "handles": [
    "relay connections",
    "rooms",
    "peer communication"
  ]
}
```

    - Item 3:
```json
{
  "id": "assigning_multiplayer_peer",
  "title": "Assigning the Multiplayer Peer",
  "content": [
    "NodeTunnel must be assigned to Godot’s multiplayer system."
  ],
  "code": {
    "language": "gdscript",
    "snippet": "multiplayer.multiplayer_peer = peer\n"
  },
  "notes": "Until this is assigned, NodeTunnel will not be used."
}
```

    - Item 4:
```json
{
  "id": "connecting_to_relay",
  "title": "Connecting to a Relay",
  "content": [
    "To use NodeTunnel, the peer must connect to a relay server."
  ],
  "code": {
    "language": "gdscript",
    "snippet": "peer.connect_to_relay(\"45.33.64.148:8080\", \"my_game\")\n"
  },
  "details": {
    "relay_address": "Points to the NodeTunnel relay",
    "app_id": "Separates your game from others using the same relay",
    "timing": "Usually called once when multiplayer starts"
  }
}
```

    - Item 5:
```json
{
  "id": "authentication",
  "title": "Authentication",
  "content": [
    "Relay connection is asynchronous.",
    "You must wait for authentication before hosting or joining rooms."
  ],
  "code": {
    "language": "gdscript",
    "snippet": "peer.authenticated.connect(func():\n  print(\"Relay authenticated\")\n)\n"
  },
  "warning": "If this signal does not fire, hosting and joining will fail."
}
```

    - Item 6:
```json
{
  "id": "hosting_room",
  "title": "Hosting a Room",
  "content": [
    "Once authenticated, a room can be hosted."
  ],
  "code": {
    "language": "gdscript",
    "snippet": "peer.host_room(true, \"My Lobby\", 4)\n"
  },
  "effects": [
    "registers a room with the relay",
    "makes this client the host",
    "allows other clients to join"
  ]
}
```

    - Item 7:
```json
{
  "id": "joining_room",
  "title": "Joining a Room",
  "content": [
    "To join an existing room, use its room ID."
  ],
  "code": {
    "language": "gdscript",
    "snippet": "peer.join_room(room_id)\n"
  },
  "notes": "Room IDs are typically obtained from a room list."
}
```

    - Item 8:
```json
{
  "id": "room_connection",
  "title": "Room Connection",
  "content": [
    "Hosting or joining is complete when the room_connected signal fires."
  ],
  "code": {
    "language": "gdscript",
    "snippet": "peer.room_connected.connect(func(room_id):\n  print(\"Connected to room:\", room_id)\n)\n"
  },
  "warning": "Do not assume hosting or joining succeeded until this signal fires."
}
```

    - Item 9:
```json
{
  "id": "disconnects",
  "title": "Disconnects",
  "content": [
    "Always handle forced disconnects."
  ],
  "code": {
    "language": "gdscript",
    "snippet": "peer.forced_disconnect.connect(func():\n  print(\"Disconnected from relay\")\n)\n"
  },
  "notes": "This signal fires when the relay disconnects the client."
}
```

