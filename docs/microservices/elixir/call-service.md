---
sidebar_position: 2
---

# Call Service

Elixir/Phoenix service for voice and video call management with WebRTC signaling.

## Overview

| Property | Value |
|----------|-------|
| **Port** | 4002 |
| **Database** | MongoDB |
| **Framework** | Phoenix 1.7 |
| **Language** | Elixir 1.15 |

## Features

- WebRTC signaling (offer/answer/ICE)
- 1:1 and group calls
- Screen sharing support
- Call recording metadata
- Call quality metrics
- TURN/STUN server integration

## WebSocket Events

```elixir
# Signaling events
"call:initiate" -> Start a new call
"call:accept" -> Accept incoming call
"call:reject" -> Reject incoming call
"call:end" -> End current call
"webrtc:offer" -> Send SDP offer
"webrtc:answer" -> Send SDP answer
"webrtc:ice_candidate" -> Send ICE candidate
```

## Call State Machine

```elixir
defmodule Call.StateMachine do
  use GenStateMachine

  # States: :idle -> :ringing -> :connected -> :ended

  def handle_event(:cast, :initiate, :idle, data) do
    {:next_state, :ringing, data, [{:state_timeout, 30_000, :timeout}]}
  end

  def handle_event(:cast, :accept, :ringing, data) do
    {:next_state, :connected, data}
  end
end
```
