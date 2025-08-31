
# Spotify → YouTube Playlist Converter (Preview UI)

A single-page static web app that lets you convert any Spotify playlist into a YouTube playlist **with a preview step**. The app:

- Reads tracks from a Spotify playlist (PKCE, client-side).
- Searches YouTube for the best matching video for each track (parallelized searches with exponential backoff).
- Shows a preview list where you can accept/skip/refine each match before creating the YouTube playlist.
- Creates a private YouTube playlist and adds the accepted videos to it.

## Files
- `index.html` — main single-page app (replace CLIENT_ID placeholders).
- `styles.css` — styling (green/red theme).
- `icons/spotify.svg` — small Spotify icon (included).
- `README_USER_SETUP.md` — personal setup instructions for you (how to register apps and configure everything).

## How it works (short)
1. User enters Spotify playlist URL and clicks **Load & Search**.
2. The app initiates Spotify PKCE flow to get permission to read the playlist.
3. The app opens a Google OAuth popup to request `https://www.googleapis.com/auth/youtube` permission.
4. The app fetches all tracks from Spotify, then runs parallel YouTube searches (with backoff) to find candidate videos.
5. You review the suggested videos, accept the ones you want, then click **Add Selected To YouTube** to create the playlist and add videos.

## Important notes / caveats
- **You must replace** the `CLIENT_ID_SPOTIFY` and `CLIENT_ID_GOOGLE` placeholders in `index.html` with your app client IDs and register the redirect URI (exact match).
- Google uses the implicit flow (client-side token). For production or long-lived use, consider moving OAuth to a lightweight backend to handle refresh tokens securely.
- YouTube Data API quotas may limit how many videos you can add quickly. The app uses delays and backoff to help avoid rate-limit errors.
- Search results are heuristic; the preview step exists so users can confirm matches (recommended!).

## Troubleshooting
- Redirect URI mismatch: ensure the redirect URI configured in both Spotify and Google exactly matches the page URL (including trailing slash if present).
- Popups blocked: allow popups for the site, or modify the code to use a redirect flow for Google.
- API quota errors: check Google Cloud Console → APIs & Services → Quotas and enable billing if you need more quota for testing large playlists.
