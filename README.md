# Devastator

Save the Earth. You have ten seconds.

![Platform](https://img.shields.io/badge/platform-macOS%2011%2B-black)
![Architecture](https://img.shields.io/badge/arch-Apple%20Silicon-blue)
![Release](https://img.shields.io/github/v/release/wells01440/devastator)
![License](https://img.shields.io/badge/license-MIT-green)
![Origin](https://img.shields.io/badge/COMPUTE!-August%201984-red)

Native macOS port of **Devastator** by David R. Arnold, from
[COMPUTE! issue 51](https://www.atarimagazines.com/compute/issue51/192_1_Devastator.php)
(C64 machine language by Gregg Peele). Sprites and rules come straight from
the original bytes. Keyboard controls replace the joystick. macOS only.

![Title screen](screenshots/title.png)

![Gameplay](screenshots/gameplay.png)

## Install

1. Download `Devastator.app.zip` from [Releases](../../releases) and unzip.
2. First launch: right-click the app, choose Open (it is unsigned). Or:
   `xattr -dc Devastator.app`

Intel Macs: build from source and set Build Settings > Architecture.

## Build

Open `Devastator/Devastator.fbeproj` in
[FutureBasic](https://www.brilorsoftware.com/fb/pages/home.html) (free) and
press Cmd-R.

## Play

The saucer's death ray fires every ten seconds. Each hit resets the clock
and scores 10. Ten hits destroy a saucer. Ten saucers — score 1000 — saves
the Earth. Only the crosshair kills; the box around it is glass.

| Key | Action |
|---|---|
| 1 / 2 | 1 or 2 player game (menu) |
| Arrows | aim |
| Space | fire, one shot per press |
| 1 / 2 / 3 | difficulty (in game) |
| W A S D | player 2 flies the saucer |
| F | player 2 jams the gunsight |
| Return | replay |
| Q | quit |
| Esc | menu |

## Fidelity

- Sprite data is byte-identical to the published listing.
- Movement clamps, fire gate, hit pause, doom clock, waves, and scoring
  follow the disassembly at $C000-$C6CB.
- The original's glitched playfield fill is redrawn as the intended
  symmetric corridor.
- F1/F3/F5 difficulty keys become 1/2/3. SHIFT/LOCK crosshair sizing
  becomes player 2's jam key.
- Sound is synthesized to approximate the SID drone, shot, sweep, and
  explosion.

## Credits

- Original game: David R. Arnold, COMPUTE! issue 51, August 1984, p. 58.
  C64 machine language: Gregg Peele, COMPUTE! Publications.
- [Article text](https://www.atarimagazines.com/compute/issue51/192_1_Devastator.php)
  | [Scanned issue](https://archive.org/details/1984-08-compute-magazine)
  | [FutureBasic](https://www.brilorsoftware.com/fb/pages/home.html)
- Port: Wells (wells01440) with Claude Code, 2026.

Original design and sprite data copyright 1984 COMPUTE! Publications, Inc.
and the authors. Non-commercial preservation project. Port code is MIT
licensed; see LICENSE.
