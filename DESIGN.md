# UNIT-01 Watcher

A dead man's switch for an always-on machine ("UNIT-01"), built as an art/demo
piece for an engineering show. A wall-mounted phone watches two heartbeats — the
machine, and its keeper — and narrates what it would do if either went silent.

This repository is **public** and is the *off-machine* half, by design: the whole
point is that detection and display keep working when UNIT-01 itself is dead. So
nothing here may depend on that machine, and nothing here may contain a secret,
address, path or identifier. The machine-side code and the full recovery runbook
live in a separate private repository.

## The core rule

Everything needed to *detect* a problem and *show* it must survive UNIT-01 being
completely off. Two independent, free vendors provide that, so no single outage
blinds the watcher:

- **Push (primary, timely): a dead-man check.** UNIT-01 pings an external check
  every few minutes. When it stops — because the machine died — the check fires.
  No inbound reachability needed, so it works even though the machine's public
  endpoint is also gone.
- **Pull (secondary, diagnostic): a scheduled job in this repo.** It probes the
  machine's public endpoint from the outside and distinguishes *"the agent is
  down but the machine is up"* from *"the machine is unreachable"*. It writes a
  sanitized `docs/status.json` for the wall page and alerts on a change for the
  worse. Scheduled runs are best-effort and can lag; the push check is what makes
  detection timely.

## Heartbeat levels

The machine reports five levels. The wall shows them; only states ride the wire,
never content:

- **Body** — machine reachable (judged here, by the pull probe)
- **Pulse** — the agent process and its event loop are alive
- **Hearing** — its inbound channels are still being polled
- **Voice** — its last outbound model call succeeded
- **Routine** — the daily jobs completed on time

## Wall states

One screen, two panels (UNIT-01 and the keeper), four combined states:

| State | Meaning |
|---|---|
| **Both alive** | green |
| **Agent down** | amber — process/loop quiet, machine still up |
| **Keeper silent** | the keeper's own panel has gone quiet |
| **Both silent** | red — machine unreachable |

## Alerting

Alerts originate off the machine and off the phone (so neither dying silences
them), and reach the keeper only. Third-party contacts are designed for but never
enabled without the keeper's explicit switch.

## Resurrection protocol (sanitized)

When the machine is presumed dead the wall shows the recovery steps as an ordered
checklist — *what* is missed and *in what order* it comes back — with no
addresses, paths or identifiers. The real runbook lives in the private repo.

See `docs/` for the wall page and the latest probe snapshot.
