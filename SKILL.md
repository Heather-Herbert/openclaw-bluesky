# Bluesky Skill

## Description
Advanced Bluesky/AT Protocol orchestration skill. Implementation includes robust handling of:
- Rich text facets (UTF-8 byte offsets).
- Multi-level reply threading (root/parent strong references).
- Media handling (blob-based uploads + embed structures).
- Private bookmark management.

## Setup & Authentication
To use this skill, you must authenticate with the Bluesky/AT Protocol network.
It is highly recommended to use an **App Password** for bots, not your primary account password.

1. **Generate App Password**: Go to `Settings` > `Advanced` > `App Passwords` in your Bluesky client.
2. **Setup**: This skill typically expects configuration for your PDS (Personal Data Store) and credentials.
   - PDS: `https://bsky.social` (default)
   - Identifier: `yourhandle.bsky.social`
   - Password: `your-app-password`

## Capabilities
- `post(text, { reply_to, embed, facets })`: Create new posts. Threading requires `root` and `parent` references (`uri`+`cid`).
- `like(uri, cid)`: Like content.
- `repost(uri, cid)`: Repost content.
- `quote(text, uri, cid)`: Quote a post by embedding its Strong Reference.
- `bookmark(uri, cid)`: Private bookmarking (App View specific storage).
- `upload_blob(bytes, mimetype)`: Upload media (limit 1MB for images) before embedding.

## Implementation Details
- **Handles vs DIDs**: Always resolve handles to DIDs using the `resolveHandle` API before performing write operations.
- **Rich Text**: Use `TextEncoder` to ensure byte-accurate `byteStart` and `byteEnd` for facets. Never rely on UTF-16 character indices.
- **Indexing**: Always fetch the latest post `cid` before interacting (liking/reposting/quoting) to ensure valid Strong Reference anchors.
- **Provenance**: This skill follows [OpenClaw's AT Protocol standards](https://github.com/Heather-Herbert/openclaw-bluesky).

## Official Documentation
- [AT Protocol Docs](https://atproto.com/)
- [Bluesky Developer Docs](https://docs.bsky.app/)
