# Quick Start Guide

This page explains how to start using **NodeTunnel** in a Godot project.

It shows:

- what NodeTunnel needs to exist
- what the setup looks like
- how connecting, hosting, and joining work

Code snippets are small and only meant to show what you should expect to see.

---

## NodeTunnel installation

NodeTunnel is available as a free addon to Godot 4.2 and onward.

1. [First, download the latest version of NodeTunnel here.](https://github.com/NodeTunnel/godot-plugin/releases/latest) **Linux users should download `.tar` to ensure permissions are kept.** All other users should use `.zip`.

2. After downloading NodeTunnel, simply decompress and drag the `nodetunnel` folder into the `addons` directory in your Godot project.

3. After installation is completed, enable the plugin in `Project Settings > Plugins > NodeTunnel > Enable`

_Note: Some file systems like to put `nodetunnel` in it's own directory, so you end up with: `nodetunnel/nodetunnel/<plugin contents>`. If you encounter problems, please ensure your directory looks like: `res://addons/nodetunnel/<plugin contents>`_

---

## Getting an application token

In order to connect to NodeTunnel's relay servers, you must first get a unique application token:

1. First, head over to [nodetunnel.io](https://www.nodetunnel.io) and register a new account if you haven't already.
2. Navigate to the dashboard page, where you can then enter details about your game, such as a name and an optional description.
3. After registering the application with NodeTunnel, copy the 15 character application token. **You'll need this later.**

_Note: If you wish to self-host, this is not required. **curtjs** created this system to prevent abuse on the public relays._

## Creating a NodeTunnel peer

NodeTunnel works by providing a custom multiplayer peer. This peer is built to behave very similarly to `ENetMultiplayerPeer`.

A peer is created like this:

```
var peer := NodeTunnelPeer.new()
```

This object handles:

- Relay connections
- Joining/hosting rooms
- Peer communication

---

## Assigning the multiplayer peer

NodeTunnel must be assigned to Godot’s multiplayer system.

```
multiplayer.multiplayer_peer = peer
```

Until this is assigned, NodeTunnel will not be used by Godot.

---

## Connecting to a relay

To use NodeTunnel, the peer must connect to a relay server.

```gdscript
peer.connect_to_relay("us_east.nodetunnel.io:8080", "my_application_token")
```

- The first parameter is the relay address. Currently, there are **2 free relay servers:**

      **US East:**

      - Address: `us_east.nodetunnel.io:8080`

      **EU Central:**

      - Address: `eu_central.nodetunnel.io:8080`

- The second parameter is your unique application token. [See here for more information.](#getting-an-application-token)

---

## Authentication

Relay connection is asynchronous. After calling `peer.connect_to_relay`, you must wait for the `peer.authenticated` signal **_before_ attempting to host or join rooms.**

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
peer.host_room(false, "")
```

`host_room` has two parameters:

- `is_public` -> `bool`:

      Setting `is_public` to `true` includes this room in `peer.get_rooms()`. Setting it to `false` hides it from this list.

- `room_metadata` -> `string`

      This is a more advanced feature of NodeTunnel. Data passed in here will be included when the room is listed. Leave it blank for a simple setup.

This:

- Registers a room with the relay
- Makes this client the host
- Allows other clients to join

---

## Joining a room

To join an existing room, use its room ID.

```gdscript
peer.join_room(room_id)
```

Room IDs are typically obtained from a room list, or shared over a messaging platform.

There is an optional `join_metadata` parameter, which can be used for passwords, max players, etc.

---

## Room connection

Hosting or joining is complete when the `room_connected` signal fires.

```gdscript
peer.room_connected.connect(
	func(room_id):
		print("Connected to room:", room_id)
)
```

Do not assume hosting or joining succeeded until this signal fires.

---

## Disconnects & Error Handling

Useful error messages will be sent from the relay server over the `error` signal:

```gdscript
peer.error.connect(
	func(msg):
		print("Relay sent error: " + msg)
)
```

This includes errors such as:

- Invalid app token
- Invalid client version
- Room ID not found

And more. **It is very important to connect to this signal.**

Additionally, there are various reasons why the relay server would forcibly disconnect a client. You can handle this case like so:

```gdscript
peer.forced_disconnect.connect(
	func():
		print("Disconnected from relay")
)
```

This signal fires when the relay disconnects the client.
