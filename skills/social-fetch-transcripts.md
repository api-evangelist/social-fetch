---
name: social-fetch-transcripts
description: Pull the spoken transcript of a video post from its URL, across platforms.
api: Social Fetch REST API
generated: '2026-09-11'
method: generated
source: openapi/social-fetch-openapi.json (transcript routes verified against the live spec)
operations:
  - GET /v1/tiktok/videos/transcript
  - GET /v1/twitter/tweets/transcript
  - GET /v1/youtube/videos/transcript
  - GET /v1/instagram/posts/transcript
  - GET /v1/facebook/posts/transcript
  - GET /v1/reddit/posts/transcript
---

# Get a video transcript from a URL

Social Fetch exposes a transcript route per platform. Give it the post/video URL
and it returns the spoken transcript.

## Steps

1. **Pick the platform route** matching the URL, e.g.
   `GET /v1/tiktok/videos/transcript` for a TikTok video or
   `GET /v1/twitter/tweets/transcript` for a video tweet.

2. **Pass the URL** as the documented query parameter (see the route's `.mdx`
   page or `/llms-{platform}.txt`).

3. **Handle non-video targets:** a URL that is not a video returns
   `400 bad_request` with code `transcript_target_not_video`. Check
   `data.lookupStatus` on 200 for `not_found` / `restricted` before using the text.

## Notes
- These are metered routes — a successful transcript costs credits.
- Keep `meta.requestId` for support; honor `Retry-After` on `503`.
