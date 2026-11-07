# Video and data rules

## One video, one folder

Use `videos/<video-id>/`. The ID is stable lowercase ASCII kebab-case, such as `restaurant-checkin-demo` or `20261107-shanghai-cafe`. Use the same ID in the manifest and file names; never repurpose an ID for another video.

Each folder must contain:

- `video.json`: structured metadata and current asset references.
- `edit-plan.md`: purpose, audience, shot/timing plan, style decisions, missing inputs, and render review.
- `subtitles/`: publishable UTF-8 ASS/SRT files.

Optional `assets/` holds publishable supporting images; ignored `media/` holds local inputs; ignored `renders/` holds local outputs. Do not commit large video/audio files. Store authorized media externally and record a durable locator without credentials. Local paths are relative to the video folder; external assets use HTTPS URLs. A `null` source means no source is available.

## File names and versions

`<video-id>__<role>__<language>__vNNN.<extension>`

Roles: `source`, `overlay`, `captions`, `music`, `thumbnail`, `render`. Language: lowercase BCP-47 tag (`zh-cn`, `en`) or `und` for non-language assets. Versions start at `v001`; never overwrite an approved deliverable. Example: `restaurant-checkin-demo__overlay__zh-cn__v001.ass`.

Use lowercase ASCII paths, no spaces, UTF-8 text, LF endings, JSON without comments, and ISO 8601 dates. Increment versions for content changes; update `video.json` to select the current version. Git records history; pin the commit SHA in render records so remote tools receive the reviewed bytes.

## Manifest contract

Copy `templates/video.json`. Keep these keys:

- `schema_version`: integer `1`; `video_id`: matches folder; `title`: human-readable.
- `status`: `draft`, `ready`, or `rendered`. `ready` requires available source and a complete plan; `rendered` requires output inspection recorded in the plan.
- `language`: lowercase language tag.
- `target`: desired duration in seconds and integer frame width/height. This is a design target, not measured source metadata.
- `source`: `null`, or an object with `location`, measured `duration_seconds`, `width`, `height`, `fps`, and `sha256`. Unknown measured properties stay `null`; do not guess hashes.
- `subtitles`: array of objects containing `role`, `format` (`ass` or `srt`), `language`, `version` (`vNNN`), and `path` relative to the video folder.
- `output`: `null` until rendered, then an object containing `location` and `sha256`.

## Timing and styling

Times refer to the final video timeline, starting at zero. Use seconds in prose/tables; ASS uses `H:MM:SS.cc`, SRT uses `HH:MM:SS,mmm`. Require `0 <= start < end <= measured final duration`. Overlaps must be intentional and documented; gaps are allowed. Overlay cards and speech captions are separate roles and may coexist.

The ASS events are authoritative for the demo: several events end half a second later than the introductory rounded table, and the final card begins at 27.50 seconds. Preserve those exact timings unless deliberately revising the plan.

ASS uses BGR color encoding. `BorderStyle=3` creates an opaque box; it is not a sticker or rounded card. Kinetic animation requires explicit ASS override tags or editor effects. Font availability and emoji rendering vary across hosts; inspect or replace unsupported glyphs with authorized image assets. Never promise rendered visual fidelity based only on a subtitle file.
