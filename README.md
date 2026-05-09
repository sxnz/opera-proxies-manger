# opera-proxies-manger
Free Windows desktop app to grab, check &amp; manage proxies — FastAPI + Next.js + Electron, supports HTTP/HTTPS/SOCKS4/SOCKS5, async mass-checking, geo detection, and TXT/CSV/JSON export.

Opera Proxies Manager follows a single-server desktop architecture -> Python backend (FastAPI + Uvicorn) acts as the sole process — it serves the REST API under /api/* and simultaneously delivers the embedded Next.js static frontend at the rooف Electron wraps this backend as a native Windows desktop app: on launch it kills any stale instance, spawns the backend executable, polls /health until ready, then loads the UI in a Chromium window. All proxy data is persisted in a local SQLite database stored in the user's AppData directory The async engine (aiohttp + asyncio Semaphore) handles hundreds of concurrent proxy fetch and check operations without blocking no separate frontend server, no cloud dependency, no runtime installation required.

-------WORKFLOW-------

1. App opens → Electron spawns backend.exe → polls /health → loads UI
2. Collect → async fetch 14+ sources in parallel → parse IP:Port → dedupe → save to DB
3. Check → semaphore(100) → test each proxy → measure latency → geo lookup → mark working/slow/dead
4. Export → filter by status/protocol/country/latency → stream TXT, CSV, or JSON
5. Files → upload list → rename / merge / dedupe / recheck / download
