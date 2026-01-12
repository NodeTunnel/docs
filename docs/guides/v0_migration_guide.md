# Migration Guide

If you are still using v0.x of NodeTunnel (written entirely in GDScript), it is **highly** recommended that you upgrade. v1.x of NodeTunnel is far superior in speed, usability, and stability. Migrating is very simple as well.

Note that this is the bare minimum to make your current game work with NodeTunnel v1. Please consider reading the rest of the docs to learn the rest of the features.

---

# Switching versions

1. In order to switch to the new version of NodeTunnel, you must first uninstall the current version. Disable the plugin, then delete it. You may have to quit the editor and do this from an external file manager.

2. Install v1.x of NodeTunnel (**see [the quick-start guide](./quick_start.md) for instructions on installation.**)

---

# Breaking changes

**`connect_to_relay(addr, port)` -> `connect_to_relay(addr, token)`**

- Address and port parameters have been combined into one string. _(example: "us_east.nodetunnel.io:8080")_
- Now requires an application token to be passed in as well. [Get this from the new NodeTunnel website](https://www.nodetunnel.io) if you wish to use the public relay servers.

**`relay_connected` -> `authenticated`**

- This is basically just a rename. **You must still wait for this signal before hosting/joining.**
- `online_id` has been removed in favor of `room_id`, and is no longer generated until a room has been created.

**`host()` -> `host(public: bool, metadata: string)`**

- To retain original functionality, just set `public` to false and `metadata` to an empty string.

**`hosting`, `joined` -> `room_connected(room_id)`**

- To simplify things, both signals have been simplified to one `room_connected` signal.
- This signal returns the room ID of the room joined, **and can be shared to other players to join the room.**

**`online_id` -> `room_id`**

- Room ID has replaced online ID. Note that the room ID will only be available **after** `room_connected` has signaled.
