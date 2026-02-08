# Socket Client Authentication

This document explains the room-based authentication flow for `socket-hub`.

## Purpose

Clients are **not allowed** to create rooms on their own.

Rooms must be provisioned by the system owner first, then clients can:
1. Join an existing room with credentials.
2. Submit claims only after successful authentication.

## Server Configuration

Set these environment variables for `socket-hub`:

- `ROOM_ADMIN_TOKEN`: owner token required to provision rooms.
- `REDIS_URL`: Redis connection used to store room credentials and room queues.
- `ROOM_PASSWORD_PBKDF2_ITERATIONS` (optional): PBKDF2 iterations for room password hashing (default: `120000`).

## Owner Provisioning API

### `POST /api/rooms/register`

Registers (or overwrites) room credentials.

Headers:

- `X-Admin-Token: <ROOM_ADMIN_TOKEN>`

Body:

```json
{
  "room_id": "client-room-1",
  "password": "strong-client-password",
  "overwrite": false
}
```

Responses:

- `200`: room credentials created (or already existed if `overwrite=false`).
- `403`: invalid admin token.
- `503`: provisioning unavailable (missing token or Redis down).

## Socket Authentication Flow

### 1. Client joins room

Emit `join_room` with both `room_id` and `password`:

```json
{
  "room_id": "client-room-1",
  "password": "strong-client-password"
}
```

On success:

- Server emits `join_room_success` to that socket:

```json
{
  "room_id": "client-room-1"
}
```

On failure:

- Server emits `auth_error`:

```json
{
  "code": "invalid_credentials",
  "message": "Invalid room credentials."
}
```

Common `auth_error.code` values:

- `bad_request`
- `invalid_credentials`
- `service_unavailable`
- `unauthorized`
- `forbidden_room`

### 2. Client submits claim

Client can emit `post_message` only after successful `join_room`.

```json
{
  "content": "Claim text here",
  "timestamp": "2026-02-08T10:00:00Z",
  "room_id": "client-room-1"
}
```

Rules:

- If socket is not authenticated, server returns `auth_error`.
- If `room_id` is different from the authenticated room, server returns `auth_error` with `forbidden_room`.
- Empty `content` is rejected with `worker_update` status `error`.

## Minimal Python Client Example

```python
import socketio

sio = socketio.AsyncClient()

@sio.event
async def join_room_success(data):
    print("joined:", data)

@sio.event
async def auth_error(data):
    print("auth failed:", data)

await sio.connect("http://localhost:8001")
await sio.emit("join_room", {"room_id": "client-room-1", "password": "strong-client-password"})
await sio.emit("post_message", {"content": "example claim", "room_id": "client-room-1"})
```

## Security Notes

- Room passwords are stored as PBKDF2 hashes with per-room random salt.
- Plain-text passwords are never stored in Redis.
- Keep `ROOM_ADMIN_TOKEN` secret and rotate it periodically.
