# Remote

A lightweight Roblox utility for managing `RemoteEvent` creation and client-server event wiring. This package centralizes remote events in `ReplicatedStorage/RemoteEvents`, prevents duplicate server-side remote names, and gives clients a safe wrapper to connect and fire server events.

## Features

- Creates `RemoteEvent` or `UnreliableRemoteEvent` on the server.
- Prevents duplicate remote names on the server.
- Client-side modules can wait for the remote to appear and connect to `OnClientEvent`.
- Provides a clean server/client API without requiring consumers to manage the `RemoteEvents` folder manually.

## Install

Install the package with wally:

```bash
wally add ptekspy/remote
```

## Usage

Require the package from your project path:

```lua
local Remote = require(path.to.Remote.src.init)
```

### Server

```lua
local remote = Remote.New("ChatMessage")
remote:FireServer("hello")
```

### Client

```lua
local remote = Remote.New("ChatMessage")
remote.OnClientEvent:Connect(function(message)
	print("Server sent:", message)
end)
remote:FireServer("hello from client")
```

## API

`Remote.New(name: string, is_unreliable: boolean?)`

- `name` — Remote event name.
- `is_unreliable` — optional boolean to create an `UnreliableRemoteEvent`.
- server-side returns the created `RemoteEvent` instance.
- client-side returns a wrapper object with `OnClientEvent.Connect()` and `FireServer()`.

## Notes

- `Remote.New()` throws on the server if a remote with the same name already exists.
- Client-side `FireClient()` and `FireAllClients()` are not supported and will throw if called.
- All events are stored under `ReplicatedStorage/RemoteEvents`.

## Contributing

This repository is intended for Roblox developers who want a minimal remote event helper with built-in duplicate detection and client-side connect/wait behavior.
