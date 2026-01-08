---
sidebar_position: 3
---

# Message Service

Elixir/Phoenix service for real-time messaging with read receipts and reactions.

## Overview

| Property | Value |
|----------|-------|
| **Port** | 4003 |
| **Database** | MongoDB |
| **Framework** | Phoenix 1.7 |
| **Language** | Elixir 1.15 |

## Features

- Real-time message delivery
- Read receipts
- Message reactions
- Message editing/deletion
- File attachments
- Threaded replies
- Message search

## Message Schema

```elixir
defmodule Message do
  use Ecto.Schema

  embedded_schema do
    field :channel_id, :string
    field :sender_id, :string
    field :content, :string
    field :type, Ecto.Enum, values: [:text, :image, :file, :system]
    field :attachments, {:array, :map}
    field :reactions, {:array, :map}
    field :reply_to, :string
    field :edited_at, :utc_datetime
    field :deleted_at, :utc_datetime
    timestamps()
  end
end
```

## Phoenix Channel

```elixir
defmodule MessageWeb.MessageChannel do
  use MessageWeb, :channel

  def handle_in("new_message", %{"content" => content}, socket) do
    message = Messages.create_message(%{
      channel_id: socket.assigns.channel_id,
      sender_id: socket.assigns.user_id,
      content: content
    })

    broadcast!(socket, "message", message)
    {:reply, {:ok, message}, socket}
  end

  def handle_in("add_reaction", %{"message_id" => id, "emoji" => emoji}, socket) do
    Messages.add_reaction(id, socket.assigns.user_id, emoji)
    broadcast!(socket, "reaction_added", %{message_id: id, emoji: emoji})
    {:noreply, socket}
  end
end
```
