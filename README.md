# Theodia Podcasts

Curated directory of Christian podcast feeds for Theodia. Each entry in
[`index.json`](./index.json) is a **feed subscription** — type `podcast-feed` —
not a packaged playlist. The feed refreshes itself, so there is nothing to
repackage when a new episode posts.

## How it is used

The app's resource manager reads `index.json` from the `main` branch and lists
every `podcast-feed` entry under the **Podcasts** tab. Installing an entry
subscribes to the feed: the app fetches the feed, stores the show and its
episodes locally, and keeps the subscription refreshed in the foreground.

## Adding an entry

1. Verify the feed before adding it:
   - The feed URL **and** its episode enclosures must be `https://`.
   - The feed must be valid RSS 2.0 or Atom with `audio/*` enclosures.
   - Each episode must carry a stable `<guid>` — the app keys episodes by
     `(podcastId, guid)`.
   - The feed must be reachable without authentication.
2. Add an entry to `index.json` with:
   - `id` — lowercase, dash-separated (see the repo naming standards).
   - `type` — `"podcast-feed"`.
   - `name` / `title` — the display name.
   - `feedUrl` — the HTTPS feed URL.
   - `description` — one or two sentences written for a listener.
   - `imageUrl` — the show artwork.
   - `author` — the publisher.
   - `category` — `"Podcast"`.
   - `version` — the entry's version string.
3. Commit and push. The entry appears in the app's resource manager under
   *Podcasts*.

## Verification commands

```bash
# Feed is reachable over HTTPS
curl -s -o /dev/null -w "%{http_code}\n" -L https://example.com/feed.xml

# Episode enclosures are audio over HTTPS
curl -sL https://example.com/feed.xml | grep -oE '<enclosure[^>]*>' | head -3
```

## License notes

Feeds are listed so listeners can subscribe from the publisher's own feed. The
directory does not re-host any audio. Most feeds are free for personal
listening; review the publisher's terms before adding a feed, and record any
restrictions in the entry's description.

## Related documentation

- [`notes/podcast-design.md`](https://github.com/ecodiallc/TheodiaApp/blob/main/notes/podcast-design.md) — the first-class podcast feature design
- [`notes/podcast-ideas.md`](https://github.com/ecodiallc/TheodiaApp/blob/main/notes/podcast-ideas.md) — the feed backlog this directory is built from
