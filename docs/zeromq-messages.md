# ZeroMQ Messages

## Sent by Client

### `parse`

#### Description

Asks the server to parse a string of Alda code.

Options include `as`, which can be one of `lisp` or `map`, indicating whether the score should be parsed into [`alda.lisp`](alda-lisp.md) code or the final map of score data. When omitted, the default behavior is to parse as `lisp` code.

#### Example

```json
{
  "command": "parse",
  "body": "piano: c8 d e f g2",
  "options": {
    "as": "map"
  }
}
```

---

### `ping`

#### Description

Pings the server to see if it's up.

#### Example

```json
{"command": "ping"}
```

---

### `play`

#### Description

Asks the server to play a string of Alda code.

Options include `from` and `to` strings, representing minute/second markings or score marker names directing the server where in the score to start/end. (When omitted, the score will default to the beginning and end of the score.)

#### Example

```json
{
  "command": "play",
  "body": "piano: c8 d e f g2",
  "options": {
    "from": "chorus",
    "to": "5:55"
  }
}
```

---

### `play-status`

#### Description

Asks a worker for its current status, e.g. parsing a score, playing a score, done.

> Note that this message is sent not to the server, but to a specific worker. To do this, you add an extra frame with the worker's address. For more details, see [ZeroMQ Architecture#Message Structure](zeromq-architecture.md#message-structure).

#### Example

```json
{"command": "play-status"}
```

---

### `stop-server`

#### Description

Tells the server to shut down.

#### Example

```json
{"command": "stop-server"}
```

---

### `status`

#### Description

Asks the server for its current status.

The response from the server is a string like `Server up (2/2 workers available)`.

#### Example

```json
{"command": "status"}
```

---

### `version`

#### Description

Asks the server for its Alda version number.

#### Example

```json
{"command": "version"}
```

## Sent by Server

> The server sends 3 kinds of messages:
>
> - Messages forwarded to/from clients and workers.
> - One-frame worker control messages like `KILL` and `HEARTBEAT`.
> - Direct JSON responses to client requests that don't need to be handled by workers.
>
> Only the last type is covered here.

### Example Response

```json
{"success": true, "body": "1.0.0-rc42", "noWorker": true}
```

## Sent by Worker

### Example Response

```json
{"success": true, "pending": false, "body": "playing"}
```

## See Also

* [ZeroMQ Architecture](zeromq-architecture.md)
