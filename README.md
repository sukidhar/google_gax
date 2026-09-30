# google_gax (EnlightenAI fork)

Fork of [`google_gax`](https://hex.pm/packages/google_gax) 0.4.0 (upstream:
`GoogleCloudPlatform/elixir-google-api`, `clients/gax` — abandoned since 2021).

## Why this fork exists

Tesla >= 1.18.3 fixed CVE-2026-48594 (decompression bomb) by making
`:max_body_size` a **required** option of `Tesla.Middleware.DecompressResponse`.
Upstream google_gax passes no options, so every response through any
`google_api_*` client raises `ArgumentError`. Tesla cannot be pinned below
1.18.3 alongside hackney 4.x (dependency conflict), and downgrading would
reintroduce five CVEs.

## Changes vs upstream 0.4.0

- `lib/google_api/gax/connection.ex`: pass `max_body_size: 64MiB` to
  `Tesla.Middleware.DecompressResponse`.
- `lib/google_api/gax/connection.ex`: multipart field name `"metadata"` as a
  string (silences an Elixir 1.20 type warning; identical wire format).

Consumed as a git dependency with `override: true`:

```elixir
{:google_gax, github: "sukidhar/google_gax", tag: "v0.4.0-enlighten.2", override: true}
```

Licensed under Apache-2.0, same as upstream (see LICENSE).
