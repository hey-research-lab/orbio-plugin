---
name: orbio-media
description: Generate images and video with Higgsfield through Orbio, paid in CREDIT. Soul photographs, Cinema Studio video shots, Genjutsu edits that keep a video's motion, and trained characters that appear in all of them. Use when asked to make an image, a video, an ad visual, a character, or to put a product or a person into an existing clip.
---

# Images and video, through Orbio

Every tool here runs on Higgsfield on Orbio's account and is paid from your Orbio
balance. No Higgsfield account or key is needed. Pick the tool for the job; the model
behind it is chosen for you.

| Job | Tool |
| --- | --- |
| A photograph, a portrait, a product shot | `media.image` |
| A video shot from a prompt, with optional image and audio references | `media.video` |
| Change who or what is in a video, or its look, keeping its motion | `media.motion` |
| A person you can put into any of the above | `media.character` |
| Collect a job, or check where it is | `media.status` |
| See the looks `media.image` and `media.motion` restyle can take | `media.styles` |

## Collecting results

`media.image` waits for its own result, usually under half a minute, and answers with
`images[].url`. A video, a motion edit or a character takes minutes, so the call answers
`202` with `status: "running"`, an `id` and `collect_with: "media.status"`. Call
`media.status` with that `id` every `retry_after_seconds` until `status` is `completed`
(the result is in `images`, `video` or `character`) or `failed` (`error` says why).
`media.status` is free and works at any balance, including zero.

A job that fails, or that Higgsfield's moderation refuses, is never charged. Outputs are
kept at least seven days (`expires_at`); download anything worth keeping.

## Characters

`media.character` trains a person from 1 to 50 photo URLs (five to twenty clear photos
from different angles work best). Use photos of yourself, or of someone who agreed to it,
and never of anyone else. Training takes about ten minutes; its `id` is the character's
id. Pass it as `character` to `media.image`, `media.video` or `media.motion`.
`media.characters` lists yours.

## Video edits

`media.motion` takes a source `video`: an https MP4 of 4 to 30 seconds that downloads
directly. `transfer` puts the person or product in `images` into the source's motion,
`swap` replaces an object in the shot with the one in `images`, and `restyle` renders the
clip in a `style` from `media.styles`. Orbio measures the source before anything is held,
so the hold is the exact price.

## Local files

Any `images` field takes https URLs or base64 `data:` URLs of a JPEG, PNG or WebP, so a
photo on disk can be sent inline. A whole call is limited to about 4 MB over the API and
1 MB over MCP, so send small images inline and host larger ones. A source video for
`media.motion` must be an https URL; Orbio copies it, up to 200 MB.

## Posting what you made

A result URL can go straight into `social.post` as `media`, so an agent can make a video and
post it in two calls.

## What it costs

| Call | CREDIT |
| --- | --- |
| `media.image`, per image | 0.003438 at 720p, 0.006188 at 1080p |
| `media.video`, 5 seconds | 2.54232 at 720p, 1.130816 at 480p |
| `media.motion`, per second of source | 0.7491 at 720p, 0.3498 at 480p, 1.7952 at 1080p |
| `media.character`, one training | 2.75 |
| `media.status`, `media.characters`, `media.styles` | free |

`media.video` costs the same with or without sound and references. `media.motion` bills
whole seconds of the source, so a 4.04 second clip bills as 5. Pass `max_cost` to cap any
call; a call quoted above it, or above what the balance can hold, is refused before
anything runs.
