# Starfy 2 v0.9.9 16 MB: SuperCard restart / save-state test (beta 1)

Test build only. Not a release. Do not distribute a patched ROM, only this `.bps`.

This test build is for the main project at https://github.com/Pat227-spc/stafy-patches (issue #3: https://github.com/Pat227-spc/stafy-patches/issues/3).

| | |
|---|---|
| Patch | `Starfy.2.Eng.v0.9.9-supercard-beta-1.bps` (532,746 bytes) |
| Apply to | Densetsu no Stafy 2 (Japan), CRC32 `EA73EB54` |
| Result | CRC32 **`523BD912`**, 16,777,216 bytes (16 MiB) |
| Patch SHA-1 | `b36e23e7211c3dc1ebd445c0ccf549dd795b68f9` |
| Patch SHA-256 | `74b8bd45736a87c8b9af557aee672ab2b56fbef48b130c3542dc1506bbd21024` |

## What changed

This is the same game as the 2026-11-16 leftover-Japanese test build (CRC `78E7991A`), with one layout change: the English variable-width font routine and its three width tables were moved earlier in the ROM (from `0x08FF6F38` / `0x08FF7000` / `0x08FFA000` / `0x08FFD000` to `0x08F36F38` / `0x08F37000` / `0x08F3A000` / `0x08F3D000`). The last used byte is now at `0xF3D058`, leaving **798,632 bytes** of `0xFF` at the end of the 16 MiB image.

The official Super Card SD client v2.71 trims that trailing `0xFF` and appends its own code: 19,520 bytes for Restart only, or 71,752 bytes for Restart plus Real Time Save. On v0.9.9 that payload crossed the 16 MiB line, which is a likely cause of the restart and save-state problems reported in issue #3. In this build the appended code ends below 16 MiB (Restart: `0x0F3D060-0x0F41CA0`, output 15,998,112 bytes; Restart+RTS: `0x0F3D060-0x0F4E8A8`, output 16,050,344 bytes).

Text, graphics, and save type (`SRAM_V113`) are unchanged. Emulator screenshots of this build are pixel-identical to `78E7991A` (0 differences on 31 + 46 + 214 golden frames). Older Starfy 2 `.sav` files from other versions can still fail to load; start a new save if a previous one does not work.

## How to patch

1. Use a clean Japanese dump of Densetsu no Stafy 2, CRC32 `EA73EB54`.
2. Apply this `.bps` with [flips](https://github.com/Alcaro/Flips) or RomPatcher JS.
3. Confirm the result is 16,777,216 bytes and CRC32 `523BD912`.

## SuperCard settings to try

This build has not been tested on a real Super Card. The checks below were done with the official SuperCardsd.exe v2.71 on a PC, then in the mGBA emulator.

1. In Super Card SD software v2.71, add the patched 16 MiB ROM.
2. Leave **Save** and **Enable saver Patch** on.
3. Turn **Restart** on. For the full test, also turn **Enable Real Time Save** on.
4. Choose Out. The written file should stay under 16 MiB (about 15.26 MiB with Restart only, about 15.31 MiB with Restart plus Real Time Save).
5. Copy that file to the Super Card SD and try, in-game:
   - a normal save and a power cycle
   - Restart (L+R+Select+A, or the client's restart control)
   - Real Time Save / load, if that option was enabled

Please tell us whether it boots, whether Restart works, and whether save states work. No rush, and thank you for testing.

mGBA can boot and save the unpatched (pre-client) 16 MiB image, but it cannot prove Super Card restart or real-time save: the client's appended code is started by the Super Card firmware, and in mGBA the restart/RTS-patched images only show a white screen. mGBA also cannot prove real SD I/O mapping (the Super Card maps SD access over the upper 16 MiB on hardware).

## Credits

This patch is an assembly of work by:

- Megahog2014 / Starfy Translation Team — the Starfy 2 project (https://github.com/Megahog2014/Densetsu_no_Starfy_2_Translation_ENG, commit `de569ee`)
- SomeUselessTranslations (AUselessMage) — Starfy 2 script work from their YouTube playthroughs (https://www.youtube.com/channel/UCxEXgKEbCgTI6jsKuzNAANw) and posts on r/thelegendarystarfy
- Luckylodeon98 — Starfy 2 graphics, VWF integration, and the 5-3 fruit puzzle (https://github.com/Luckylodeon98/Densetsu_no_Starfy_2_Translation_ENG, commit `dbfd7cc`)
- RayCarrot and Normmatt — Starfy 2 VWF code
- EASTWOOD2002 — English "The Legendary Starfy" logo concept (https://forums.launchbox-app.com/topic/70667-more-cleaned-up-media/page/17/#comment-441299)

Maintained here by Pat227.
