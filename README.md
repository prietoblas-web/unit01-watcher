# unit01-watcher

Off-machine half of a dead man's switch for an always-on machine ("UNIT-01"), an
art/demo piece. **Public repo — no secrets, addresses, paths or identifiers.**

- `DESIGN.md` — what it is and how it survives the machine dying.
- `docs/` — GitHub Pages wall screen + the latest sanitized probe snapshot (`status.json`).
- `.github/workflows/probe.yml` — scheduled pull probe → `status.json` + alert on change.
- `.github/workflows/keepalive.yml` — keeps the schedule from being disabled after 60 days idle.

All machine addresses and tokens are provided to the probe as **GitHub Actions
secrets**, never committed. See the private repo for setup.
