# NodeTunnelPeer

**Inherits:** [MultiplayerPeer](https://docs.godotengine.org/en/stable/classes/class_multiplayerpeer.html#class-multiplayerpeer)

A MultiplayerPeer implementation that routes data through a remote relay server.

## Description

This is the heart of NodeTunnel. A MultiplayerPeer implementation that should be passed to [MultiplayerAPI.multiplayer_peer](https://docs.godotengine.org/en/stable/classes/class_multiplayerapi.html#class-multiplayerapi-property-multiplayer-peer) after connecting to a remote relay server. Once successfuly connected, operations like hosting and joining rooms are made possible.

## Properties

- [room_id](#room_id-string)`: String`
- [join_validation](#join_validation-callablejoin_metadata-string)`: Callable`

## Methods

- [connect_to_relay](#connect_to_relayrelay_address-string-app_token-string-error)`(relay_address: String, app_token: String) -> Error`
- [host_room](#host_roompublic-bool-metadata-string-error)`(public: bool, metadata: String = "") -> Error`
- [join_room](#join_roomroom_id-string-metadata-string-error)`(host_id: String, metadata: String = "") -> Error`
- [update_room](#update_roommetadata-string-error)`(metadata: String) -> Error`
- [get_rooms](#get_rooms-error)`() -> Error`

## Signals

- [authenticated](#authenticated)`()`
- [error](#errorerror_msg-string)`(error_msg: String)`
- [room_connected](#room_connected)`()`
- [rooms_received](#rooms_receivedrooms-arrayvariant)`(rooms: Array<Variant>)`
- [forced_disconnect](#forced_disconnect)`()`

---

## Property Descriptions

### **`room_id: String`**

The room ID that the client is currently in. Will be a blank string until [room_connected](#room_connected) emits.

### **`join_validation: Callable(join_metadata: String)`**

Used to prevent unwanted peers from connecting to the room. Returning `true` in the lambda will allow the connecting peer to join, while returning `false` will prevent the connection.

`join_metadata` is data sent from the joining client when they call [join_room](#join_roomroom_id-string-metadata-string-----error).

**Example:** Password-protected room:

```gd
var room_password = "123"

peer.join_validation = func(join_metadata) -> bool:
    var jm = JSON.parse_string(join_metadata)
    var j_pwd = str(jm.get("password", ""))
    # true if the passwords match, false if they don't.
    return j_pwd == room_password
```

_Note: Only the host player's join validation function take effect_

---

## Method Descriptions

### **`connect_to_relay(relay_address: String, app_token: String) -> Error`**

Connects the client to the given relay address using a specific application token. This must be done before joining or hosting rooms.

_See: [authenticated](#authenticated) for usage example_

### **`host_room(public: bool, metadata: String = "") -> Error`**

Hosts a room with optional metadata, such as current players, max players, etc. Cannot be called before [authenticated](#authenticated).

Metadata is only useful if the room is public. If the room is not public, it will not appear in [rooms_received](#rooms_receivedrooms-arrayvariant).

### **`join_room(room_id: String, metadata: String = "") -> Error`**

Joins a room with optional metadata, such as password, installed mods, etc. Cannot be called before [authenticated](#authenticated).

May result in [error](#errorerror_msg-string) if the room ID is invalid or not found.

### **`update_room(metadata: String) -> Error`**

Updates a room with new metadata, such as a new player count, required plugins, etc. Cannot be called before [room_connected](#room_connected). **Only the room host can call this.**

### **`get_rooms() -> Error`**

Sends a request to the relay server for a list of rooms. The reponse is handled in [rooms_received](#rooms_receivedrooms-arrayvariant).

_Note: This is likely to be changed in a future version_

---

## Signal Descriptions

### **`authenticated()`**

Emitted when the client has successfuly connected to the relay server. Requires [connect_to_relay](#connect_to_relayrelay_address-string-app_token-string---error) to be called first.

**Example:** Connect to a relay server:

```gd
func _ready():
    var peer = NodeTunnelPeer.new()
    peer.connect_to_relay(address, app_id)
    multiplayer.multiplayer_peer = peer

    await peer.authenticated
```

---

### **`error(error_msg: String)`**

Emitted when the relay server sends an error to the client in response to something.

**Example:** Print any error messages:

```gd
peer.error.connect(
    func(error_msg):
        print("Error from relay: ", error_msg)
)
```

---

### **`room_connected()`**

Emitted when the client has successfully connected to a room, whether they hosted or joined.

**Example:** Host, then print the room ID:

```gd
peer.host_room()
await peer.room_connected
print("Room ID: ", peer.room_id)
```

---

### **`rooms_received(rooms: Array<Variant>)`**

Emitted when the client receives a room list. This is triggered after [get_rooms](#get_rooms---error) is called.

Array elements look like this:

```json
{
  "id": "RandomRoomId",
  "metadata": "Some info about the room"
}
```

**Example:** Print the available rooms:

```gd
peer.get_rooms()
var rooms = await peer.rooms_received()

for r in rooms:
    print("ID: ", r.id)
    print("Metadata: ", r.metadata)
```

---

### **`forced_disconnect()`**

Emitted when the relay server forcibly disconnects the client from the server. Most commonly happens when the room host disconnects.

**Example:** Log a warning:

```gd
peer.forced_disconnect.connect(
    func():
        push_warning("Forcibly disconnected from relay server.")
)
```
