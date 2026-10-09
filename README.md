# Bully PS2 — 16:10 Aspect Ratio Patch

A PNACH patch for **Bully (USA / NTSC-U)** that adjusts the 3D camera projection and native interface scaling for a **16:10 display area**.

**Community test release — revision 4.** Tested on ARMSX2 for macOS with a MacBook Air M4. This is a game-code patch, not a Mac-specific executable modification; other compatible emulators and platforms remain untested.

## Compatibility

| Item | Tested version |
| --- | --- |
| Game | Bully, PlayStation 2, USA / NTSC-U, disc version 1.02 |
| Serial | `SLUS-21269` |
| Emulator-reported CRC | `28703748` |
| Emulator | ARMSX2 macOS, build `112cd677b4` |
| Renderer / hardware | Metal / MacBook Air M4 |

PAL, NTSC-J, other CRCs, and other emulators have **not** been validated. Do not apply these addresses to a different game executable.

## Download

Download `SLUS-21269_28703748.pnach` from the [revision 4 release](https://github.com/MatthewBlenk/bully-ps2-16x10/releases/tag/revision-4). The ZIP includes the patch and installation instructions.

## Install in ARMSX2

1. Open **Tools → Open Data Directory…** and locate the `patches` folder.
2. Copy `SLUS-21269_28703748.pnach` into it. Back up any existing file with the same name before replacing it. This download does not include other optional patches that may be in your existing file.
3. In Bully's per-game patch settings, enable **Bully 16:10 Projection v3**. The group name was retained during development to support live reload; the file itself is **revision 4**.
4. Disable nemesis2000's **Widescreen 16:9** patch and automatic widescreen patches for this game. Keep other projection or interface scaling patches disabled when testing.
5. In the per-game graphics settings, set aspect ratio to **Auto**, vertical stretch to **100%**, and all cropping to **0**. The PNACH requests `16:10` automatically.
6. **Inside Bully, select Wide (16:9).** The patch converts this native mode to 16:10. The in-game option's label will still say 16:9.

No ISO modification, game executable replacement, or additional library is required.

## Save states and live reload

You can use **Load State** to resume gameplay. Check that the game's internal setting is **Wide (16:9)** afterward.

If installing or updating while the game is running, use **Tools → Reload Cheats/Patches**. Then switch the in-game aspect ratio to 4:3 and back to 16:9 once to rebuild cached interface values.

Old save states can restore instructions from previously enabled patches. If the result looks incorrect, try a recent state created with this revision, or use a clean boot for diagnosis. Save normal game progress before restarting.

## What has been checked

- The main camera's view-window ratio was measured at approximately **1.6**, matching 16:10.
- The clock and minimap appeared circular in the inspected gameplay.
- The tester reported good results during gameplay on the tested setup.

There has been no complete playthrough covering every mission, menu, cutscene, minigame, or special camera. Prerendered content may require separate handling. Screenshots for visual comparison are not included in this release.

The patch preserves the native mode's vertical field of view and reduces its horizontal view window by 10%. A narrower horizontal framing than native 16:9 is expected when displaying 16:10 with correct proportions. It does not reproduce every camera adjustment made by nemesis2000's 16:9 patch.

## Technical notes

The patch modifies the camera view-window construction and the native UI scale initialization for this specific executable:

- Camera construction: `0x0012449C`–`0x001244AC`.
- Float32 constants in verified executable padding: `0x00124578` (`5/6`) and `0x0012457C` (`0.9`).
- UI initialization paths near `0x0020C010` and `0x004A9D10`.
- UI scale factors at `0x005E09B0`, `0x005E09B8`, `0x005E09C0`, and `0x005E09C8`.

The native widescreen UI factor of `0.75` is replaced with float32 `5/6`, with inverse scaling `1.2`. Both UI initialization branches are adjusted, but **Wide (16:9) is required for the intended camera behavior**.

Original instructions were checked against the tested ELF. No fixed scene-dependent camera pointer is patched.

## Credits

**Author: MatthewBlenk.** Developed with AI assistance (OpenAI Codex) for implementation and executable analysis.

Historical widescreen research by **Arapapa and El_Patas** informed the camera-routine investigation. See the [PCSX2 widescreen patch discussion](https://forums.pcsx2.net/Thread-PCSX2-Widescreen-Game-Patches?page=527).

This release is a separate 16:10 implementation. It does not include nemesis2000's 16:9 patch, which is available in the [PCSX2 patch repository](https://github.com/PCSX2/pcsx2_patches/blob/main/patches/SLUS-21269_28703748.pnach). No claim is made that this is the first or only Bully 16:10 patch.

Only the patch and documentation are distributed. No game ISO, game executable, BIOS, memory card, or save data is included.

## Report an issue

Please include:

- Emulator version/build and platform.
- Game serial and CRC.
- In-game aspect ratio and emulator display settings.
- Scene or mission affected, and other enabled patches.
- Whether you started from a save state or a clean boot.
- A screenshot or same-scene comparison, ideally including a circular object.

Use the repository's [Issues page](https://github.com/MatthewBlenk/bully-ps2-16x10/issues). Testing on other platforms is welcome; please distinguish verified results from assumptions.
