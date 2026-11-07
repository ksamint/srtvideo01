# srtvideo01

Agent-readable video briefs, edit plans, and publicly hosted subtitle files.

## Start here

- Agents: read [AGENTS.md](AGENTS.md), then the video's `video.json` and `edit-plan.md`.
- Follow [naming and data rules](docs/video-rules.md) for every video.
- Copy [templates/video.json](templates/video.json) and [templates/edit-plan.md](templates/edit-plan.md) to start a video.
- [Restaurant check-in demo](videos/restaurant-checkin-demo/edit-plan.md) contains the supplied 30-second overlay example. No source video or finished render is included.

## Public subtitle hosting

After pushing, use a raw HTTPS URL (the GitHub file-view page is not a subtitle URL):

```
https://raw.githubusercontent.com/ksamint/srtvideo01/main/videos/restaurant-checkin-demo/subtitles/restaurant-checkin-demo__overlay__zh-cn__v002.ass
```

For reproducible rendering, replace `main` with the full commit SHA. Fetch the URL without authentication and confirm it returns subtitle text before submitting it to a render tool. The tool must support ASS styling; SRT carries timing and text but not these styles. Raw GitHub availability to a particular provider must be tested there.

All committed content is public. Keep private footage and credentials outside this repository. GitHub Actions must stay disabled; run checks and rendering locally.
