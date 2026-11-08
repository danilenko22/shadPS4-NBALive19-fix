# NBA LIVE 19 fix for shadPS4

Unofficial [shadPS4](https://github.com/shadps4-emu/shadPS4) build with fixes for **NBA LIVE 19 (CUSA11355)**.

> Not affiliated with or supported by the shadPS4 team. Please do not report problems with this build upstream.

**Download:** [latest release](../../releases/latest) · Online play project: [nba19-online-servidor](https://github.com/danilenko22/nba19-online-servidor)

## What it fixes
| Problem | Cause | Change |
|---|---|---|
| Stuck forever on "LOADING" | On Windows, `TimedMutex::try_lock_until` returned timeout without trying the lock when the deadline had already passed. The game uses a 0 µs timed lock as a trylock. | `kernel/sync/mutex.h`: try `try_lock()` once when the deadline has passed (like `std::timed_mutex`). |
| Sparkles / "fire" on hair, skin, clothes and floor (random per session) | BC6H reflection cubemaps only have data down to 4x4; the 2x2/1x1 mips contained garbage. | `texture_cache/image_view.cpp`: sampling views of block-compressed textures end at the last full-block mip. |
| Crash with "Out of order flip IRQ" (mostly with `vblank_frequency: 120`) | A flip EOP IRQ can arrive for a buffer that is no longer pending. | `videoout/video_out.cpp`: log a warning and ignore it instead of aborting. |
| — | Compute shader LDS bounds | Pending upstream PR [#4796](https://github.com/shadps4-emu/shadPS4/pull/4796), adapted. |

Also includes dedicated game server support and deterministic reciprocals for online play.

## Recommended config
`%APPDATA%\shadPS4\custom_configs\CUSA11355.json`: `connected_to_network: false` (EA servers are closed) and `vblank_frequency: 120` (60 FPS replays and cutscenes).

## Source
- `source/nba19-fix.patch`: full diff against shadPS4 `b72489d`.
- Full source tree: [`nba19-fix`](https://github.com/danilenko22/shadPS4/tree/nba19-fix) branch of my shadPS4 fork.

## License
GPL-2.0-or-later, same as shadPS4 (see [LICENSE](LICENSE)). No game files, firmware, system modules (`.sprx`) or keys are included. You need your own copy of the game.
