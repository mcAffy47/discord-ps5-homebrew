# Discord PS5 Homebrew — maintainers wanted

**Experimental. Development paused; community continuation welcome.**

An unofficial native PS5 homebrew developed by AFFrad with AI assistance. The goal is controller-operated Discord voice without PSN. **That goal is not yet complete:** current audio requires a Windows PC running the official Discord client and the Python companion. Select the real server and channel in Windows Discord.

Forks and contributions are welcome under the included GPL license. Open an issue if you want to take over maintenance. No affiliation with Sony or Discord.

## Status: V21 Voice UI Beta

- Native PS5 executable, title ID `PPSA12360`, media category.
- Discord-inspired native and browser interfaces, microphone/headphone mute and volume.
- Controller navigation and OPTIONS diagnostics implemented.
- Local PS5 web UI at `http://<PS5-IP>:4747`; Windows companion UI at `http://127.0.0.1:8765`.
- Bidirectional PS5/Windows audio was confirmed by the author in earlier versions.
- **V21 itself has not been tested on PS5 hardware.** Native compilation and build integrity checks passed; 23 host test programs passed. Host simulations do not establish firmware compatibility or real device behavior. QR matrix generation passed; automatic image decoding was skipped.
- Optional device authorization code flow is implemented but only mock-tested. It does not join a Discord channel.

![V21 host-rendered layout preview, not a PS5 screenshot](previews/ps5_v21.png)

The preview approximates the layout using a host SVG renderer. Its font rendering differs from the native bitmap renderer.

## Run the prebuilt app

Download the V21 release package, extract it completely, and read `START_HIER.txt`. Close the old app before replacing its complete `PPSA12360` directory through your existing homebrew loader workflow. Back up the old app directory first. Use `PC/start_windows.cmd` on Windows. Python 3.9+ is required; audio setup is documented in the package.

This is a homebrew folder distribution, **not a PKG installer**. No firmware update is included or required by this package.

## Build from source

The native build uses the public PS5 native app boilerplate and payload SDK. In a suitably configured Linux/WSL environment:

```sh
bash build_standalone.sh
```

This clones a pinned boilerplate checkout into `~/discord-ps5-build` unless `DISCORD_BUILD_ROOT` is set. It runs host tests before building. The wrapper needs Git, Python 3, Clang/LLVM, Make, Ninja and the upstream build dependencies; it is not a complete OS/toolchain installer. Network access is needed to obtain dependencies.

The prebuilt V21 was compiled with Ubuntu Clang 18.1.3 and PS5 Payload SDK v0.42. See `BUILD_MANIFEST.json` for source pins and binary hash. `BUILD_WINDOWS.cmd` instead uses an already configured WSL checkout at `~/ps5-native-app-boilerplate`.

```sh
python3 tools/run_tests.py
```

Tests run sequentially because local fixtures share ports. Run them with Clang available in PATH. `TEST_RESULTS.json` records the previously executed V21 checks, not a new hardware test.

## What to work on next

1. Establish hardware startup/controller/audio behavior for this exact binary.
2. Diagnose TCP/UDP bind failures seen on the author's 13.60 setup.
3. Investigate a supported path to actual Discord voice join/leave and authentication.
4. Improve audio latency, jitter handling and LAN transport security.
5. Investigate background audio while a game runs; not proven today.

No native Discord gateway/voice session, Opus transport, or Discord voice encryption stack is implemented by the current LAN bridge. Do not describe the app as a standalone Discord client.

## Documentation

- [German instructions](README_DE.md)
- [Developer handover](docs/HANDOVER.md)
- [German project history and findings](docs/PROJEKTSTAND_DE.md)
- [Contribution guide](CONTRIBUTING.md)
- [Third-party sources](THIRD_PARTY_NOTICES.md)
- [License](LICENSE)

Keep PS SAVEDATA, Firefox PS5 and other projects separate from this project. No credentials are required to browse or test the UI. Never commit Discord client secrets, tokens or private configuration.
