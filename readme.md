🧭 Mental model (pin this first)
Jellyseerr (what you want)
        ↓
Radarr / Sonarr (decide what to download)
        ↓
Prowlarr (find torrents)
        ↓
Transmission (download)
        ↓
Jellyfin (watch)


You do NOT manually search torrents day-to-day.
You request content → automation does the rest.

✅ STEP 0 — Start everything

From the folder with your compose:

docker compose up -d


Check everything is running:

docker compose ps

✅ STEP 1 — Jellyfin (media player)

Open Jellyfin
👉 http://localhost:8096

Create admin user

Add libraries:

Movies → /media/videos/movies

TV Shows → /media/videos/shows

Anime (optional) → /media/videos/anime

Finish setup

⛔ Jellyfin does not download anything.
It only reads files once they exist.

✅ STEP 2 — Transmission (downloader)

Open Transmission
👉 http://localhost:9091

Set download directory:

/downloads


(Optional) Set username/password

Leave it running

This is the only thing that actually downloads torrents.

✅ STEP 3 — Prowlarr (indexers ONLY)

Open Prowlarr
👉 http://localhost:9696

3.1 Add indexers

Go to Indexers → Add Indexer

Add 2–5 public indexers max

If Cloudflare protected → enable FlareSolverr

⚠️ Do NOT add dozens. Fewer = more reliable.

3.2 Add download client

Settings → Download Clients → Add → Transmission

Host: localhost

Port: 9091

Test → ✅ Save

3.3 Add Apps (THIS IS THE KEY)

Settings → Apps

Add:

➕ Sonarr

URL: http://localhost:8989

API key: from Sonarr

Sync level: Full Sync

Test → Save

➕ Radarr

URL: http://localhost:7878

API key: from Radarr

Sync level: Full Sync

Test → Save

👉 After this:

Never add indexers inside Sonarr/Radarr

Prowlarr pushes them automatically

✅ STEP 4 — Sonarr (TV shows)

Open Sonarr
👉 http://localhost:8989

4.1 Media folders

Settings → Media Management → Root Folders

Add:

/shows


(and /anime if you want)

4.2 Download client

Settings → Download Clients

Transmission should already be there (from Prowlarr).
If not:

Host: localhost

Port: 9091

4.3 Done

Do not touch indexers here.

✅ STEP 5 — Radarr (movies)

Open Radarr
👉 http://localhost:7878

5.1 Media folder

Settings → Media Management → Root Folders

Add:

/movies

5.2 Download client

Same as Sonarr (Transmission via localhost).

✅ STEP 6 — Jellyseerr (how you actually use it)

Open Jellyseerr
👉 http://localhost:5055

6.1 First-time setup

Connect to Jellyfin

Connect to Sonarr

Connect to Radarr

6.2 Daily usage

Search a movie or TV show

Click Request

Done

Automation kicks in:

Sonarr/Radarr pick a release

Prowlarr finds torrents

Transmission downloads

Jellyfin sees files automatically

🎬 How you ACTUALLY watch something

Open Jellyseerr

Request content

Wait for download

Open Jellyfin

Watch

That’s it.

❌ Things you should NOT do anymore

❌ Add Torznab URLs in Sonarr/Radarr

❌ Manually download torrents

❌ Touch categories

❌ Compare with phone VPN behavior

🧪 If something doesn’t download

Check in this order:

Prowlarr → Indexers → Test

Prowlarr → Apps → Test Sonarr/Radarr

Transmission → Is a torrent added?

Sonarr/Radarr → Activity → Queue

https://trash-guides.info/Radarr/radarr-setup-quality-profiles-french-fr/#formats-audio-avances-et-hdr
https://trash-guides.info/Sonarr/sonarr-setup-quality-profiles-french-fr/

docker exec seek-n-seed-recyclarr-1 recyclarr sync

ourvir port       - "51413:51413" # transmission peer
      - "51413:51413/udp"

mettre preferred language a any dans les profiles recyclarr dans radarr