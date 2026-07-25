# Moonlight Streaming Core Library

> [!IMPORTANT]
> This is the protocol fork shared by [fxgl/moonlight-qt](https://github.com/fxgl/moonlight-qt)
> and [fxgl/Sunshine](https://github.com/fxgl/Sunshine). Use the upstream
> [moonlight-stream/moonlight-common-c](https://github.com/moonlight-stream/moonlight-common-c)
> unless you need the remote-desktop extensions described below.

## Why this fork exists

This fork carries the negotiated protocol support required for focus-aware clipboard synchronization between the fxgl Moonlight and Sunshine forks:

- `LI_FF_CLIPBOARD_SYNC` capability negotiation keeps the extension opt-in.
- Reliable client-to-host UTF-8 clipboard messages update the Sunshine system clipboard.
- Reliable focus-gained and focus-lost messages delimit the interval Sunshine uses to detect a host-side clipboard change.
- Encrypted host-to-client control messages deliver changed clipboard text to Moonlight.
- A public callback exposes received clipboard text to the Moonlight application.

Clipboard messages are text-only and limited to 60 KiB. Peers that do not negotiate the capability do not send or process these messages. This repository is pinned as a submodule by both application forks and should be updated in lockstep with them.

Moonlight-common-c contains the core GameStream client code shared between [Moonlight](https://moonlight-stream.org) clients, including [Moonlight PC](https://github.com/moonlight-stream/moonlight-qt), [Moonlight Android](https://github.com/moonlight-stream/moonlight-android), [Moonlight iOS](https://github.com/moonlight-stream/moonlight-ios), and [Moonlight Embedded](https://github.com/moonlight-stream/moonlight-embedded).

If you are implementing your own Moonlight game streaming client that can use a C library, you probably want the code here.

## Note to Developers

Moonlight-common-c requires the _specific_ version of ENet that is bundled as a submodule. This version has changes required for IPv6 compatibility and retransmission reliability, among other things. These are breaking API/ABI changes which make Moonlight-common-c incompatible with other versions of the ENet library. Attempting to runtime link to another libenet library will cause your client to crash when connecting to recent versions of GeForce Experience.
