# Tessera modifications to Mosh

This branch (`tessera-ios`) of `bambouville/mosh` is a fork of upstream Mosh at
the **`mosh-1.4.0`** release (upstream commit
`bc73a26316ede2a79259d859f8ee309b32412420`, "Bump version to 1.4.0"). It carries
the Tessera-specific patches described below.

This file is the GPLv3 §5(a) "changed files" notice for the source that
accompanies the Tessera iOS app: it records that these files were modified from
upstream, by whom, and when.

- **Modified by:** Bambouville Inc. (the Tessera copyright holder)
- **Modification date:** 2026-04-26
- **Base:** Mosh 1.4.0 (`mosh-1.4.0` / `bc73a26`)
- **Patched commit:** `cc7b072ee1c3d2a66fd221c43ab215c6554d6237`

## Changes from `mosh-1.4.0`

### `src/terminal/terminaldisplay.cc` — sync application-cursor-keys mode (DECCKM)

`Display::new_frame` now emits `ESC [ ? 1 h` / `ESC [ ? 1 l` when the
application-cursor-keys mode (DECCKM) differs from the previous frame (or on the
first frame), mirroring the adjacent bracketed-paste handling. Mosh transmits
terminal state as frame diffs; without syncing this bit a client that
reconnects or roams onto a live session could be left in the wrong cursor-key
mode, so the arrow keys emitted the wrong escape sequences. *(Compiled into the
Tessera app.)*

### `src/network/transportsender-impl.h` — anchor retransmits during shutdown

`TransportSender::update_assumed_receiver_state` now, while a shutdown is in
progress, anchors the assumed receiver state to the oldest known-acknowledged
state instead of an optimistic unacknowledged one. This prevents the peer from
discarding every shutdown packet and never learning that the session ended.
*(Compiled into the Tessera app.)*

### `src/frontend/mosh-server.cc` — whitespace / formatting only

Reflowed the `serve()` signature across multiple lines and removed stray
blank/trailing whitespace. No behavioural change, and `mosh-server` is **not**
compiled into the Tessera iOS app (which builds only the client and protocol
layers).

## Provenance and upstreaming

The two functional fixes look upstreamable; filing them against
`mobile-shell/mosh` is the eventual route back to a pristine upstream tag. Until
then, this branch is the complete corresponding source for Mosh as distributed
in Tessera.
