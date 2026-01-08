---
sidebar_position: 1
---

# Presence Service

Elixir/Phoenix service for real-time user presence and status tracking.

## Overview

| Property | Value |
|----------|-------|
| **Port** | 4001 |
| **Database** | MongoDB |
| **Framework** | Phoenix 1.7 |
| **Language** | Elixir 1.15 |

## Features

- Real-time online/offline status
- Typing indicators
- Custom status messages
- Last seen tracking
- Multi-device presence
- Phoenix PubSub broadcasting

## Phoenix Channels

```elixir
defmodule PresenceWeb.PresenceChannel do
  use PresenceWeb, :channel

  def join("presence:" <> user_id, _params, socket) do
    send(self(), :after_join)
    {:ok, assign(socket, :user_id, user_id)}
  end

  def handle_info(:after_join, socket) do
    {:ok, _} = Presence.track(socket, socket.assigns.user_id, %{
      online_at: inspect(System.system_time(:second)),
      device: socket.assigns.device
    })
    push(socket, "presence_state", Presence.list(socket))
    {:noreply, socket}
  end

  def handle_in("typing_start", %{"channel_id" => channel_id}, socket) do
    broadcast!(socket, "typing", %{
      user_id: socket.assigns.user_id,
      channel_id: channel_id
    })
    {:noreply, socket}
  end
end
```

## API Endpoints

```http
GET  /api/presence/users/{userId}
GET  /api/presence/users/{userId}/status
PUT  /api/presence/users/{userId}/status
GET  /api/presence/channel/{channelId}
```

## Status Types

```elixir
@status_types [:online, :away, :busy, :offline, :invisible]

defmodule Presence.Status do
  defstruct [
    :user_id,
    :status,
    :custom_message,
    :emoji,
    :clear_after,
    :last_seen_at,
    :devices
  ]
end
```
