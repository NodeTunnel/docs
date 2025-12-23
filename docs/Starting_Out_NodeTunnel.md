# Welcome to NodeTunnel

This page explains how to start using **NodeTunnel** in a Godot project.

It shows:
- what NodeTunnel needs to exist
- what the setup looks like
- how connecting, hosting, and joining work

Code snippets are small and only meant to show what you should expect to see.

---

## NodeTunnel installed

NodeTunnel must already be available in your project.

You should be able to reference the class:

```gdscript
var peer := NodeTunnelPeer.new()
```

If this line errors, NodeTunnel is not installed correctly.

---

## Creating a NodeTunnel peer

NodeTunnel works by providing a custom multiplayer peer.

A peer is created like this:

```gdscript
var peer := NodeTunnelPeer.new()
```

This object handles:
- relay connections
- rooms
- peer communication

---

## Assigning the multiplayer peer

NodeTunnel must be assigned to Godot’s multiplayer system.

```gdscript
multiplayer.multiplayer_peer = peer
```

Until this is assigned, NodeTunnel will not be used.

---

## Connecting to a relay

To use NodeTunnel, the peer must connect to a relay server.

```gdscript
peer.connect_to_relay("45.33.64.148:8080", "my_game")
```

Notes:
- The relay address points to the NodeTunnel relay
- The app ID separates your game from others using the same relay
- This is usually called once when multiplayer starts

---

## Authentication

Relay connection is asynchronous.

You must wait for the authenticated signal before hosting or joining rooms.

```gdscript
peer.authenticated.connect(func():
	print("Relay authenticated")
)
```

If this signal does not fire, hosting and joining will fail.

---

## Hosting a room

Once authenticated, a room can be hosted.

```gdscript
peer.host_room(true, "My Lobby", 4)
```

This:
- registers a room with the relay
- makes this client the host
- allows other clients to join

---

## Joining a room

To join an existing room, use its room ID.

```gdscript
peer.join_room(room_id)
```

Room IDs are typically obtained from a room list.

---

## Room connection

Hosting or joining is complete when the `room_connected` signal fires.

```gdscript
peer.room_connected.connect(func(room_id):
	print("Connected to room:", room_id)
)
```

Do not assume hosting or joining succeeded until this signal fires.

---

## Disconnects

Always handle forced disconnects.

```gdscript
peer.forced_disconnect.connect(func():
	print("Disconnected from relay")
)
```

This signal fires when the relay disconnects the client.
