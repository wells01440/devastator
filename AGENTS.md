# Agent notes for the Devastator port

FutureBasic port of Devastator (COMPUTE! issue 51, August 1984, by David R.
Arnold; C64 machine language portion by Gregg Peele). The original C64
program is in ../devastator-original (BASIC loader + MLX-format machine
language, $C000-$C6CB). The port lives in Devastator/. The sequel,
Devastator 2, is developed separately in ../devastator2.

## FutureBasic lessons

Project structure
- A project is `<Name>.fbeproj/data.fbproj` (an ordered include list under
  section comments: Headers, Resources, Globals, Includes, Main) plus source
  files. Stream order equals file order; a function or constant must appear
  in the stream before its first use.
- Convention: thin `.main` holding `HandleEvents`, code in `.incl` files.
- The IDE owns data.fbproj while the project is open and rewrites it from
  memory on quit. To restructure: quit FutureBasic, edit data.fbproj on
  disk, relaunch with the .fbeproj.

Source
- Source files require CR or CRLF line endings; both translate. An LF-only
  file parses as a single line, so a leading // comments out the entire
  file, producing "Unknown function" errors downstream. This repo uses CRLF
  so GitHub renders the sources; .gitattributes disables eol conversion.
- Reserved words seen breaking builds: `off`, `bit`. Also avoid `frame`,
  `box`, `put`, `col` as identifiers.
- The `sound` statement alone does not link the Sound module ("_SoundStop"
  undefined at link). One explicit Sound toolbox call fixes detection:
  `fn SoundExists( NULL )`. The statement accepts an in-memory CFDataRef
  WAV.
- CocoaUI constructors return autoreleased objects; retain before storing
  in globals. Use `fn CFRetain` for CF types; `fn ObjectRetain` is typed
  `id` and raises clang incompatible-pointer warnings on CFDataRef.
- Header includes resolve as `include "CocoaUI/Sound.incl"`.

Game patterns
- Keyboard: `subclass view` for the canvas, then
  `ViewSetAcceptsFirstResponder( tag, YES )` and
  `WindowMakeFirstResponder( wndTag, viewTag )`; handle `_viewKeyDown` /
  `_viewKeyUp` with `DialogEventSetBool( YES )` on keyDown.
  `_windowKeyDown` alone never fires. `fn EventKeyCode` gives the macOS
  virtual key code.
- Drawing: in `_viewDrawRect` get the context with
  `fn GraphicsContextCurrentCGContext`; CGContext* calls work directly.
- Game loop: `timerbegin , 0.0167, YES ... timerend` runs NSTimer on the
  main run loop.

Building and testing
- No CLI compiler. Build with the IDE: open the .fbeproj, Command > Run
  (Cmd-R). Errors appear in the "Errors/Warnings" window, full output in
  "Build Log". Built app lands in build_temp_<name>/ArmDir/ and is copied
  next to the project as <Name>.app.
- Driving the IDE with cua-driver: a freshly launched, backgrounded
  FutureBasic ignores hotkey cmd+r until it has had a key window; AXPick
  the Command menu bar item, re-snapshot, then AXPick "Run". A "Multiple
  FutureBasic apps" startup alert can silently block quit.
- The IDE picks up on-disk source edits on rebuild; no reopen needed.

## Original game rules (from the COMPUTE! article and disassembly)

Crosshair must overlap the alien ship pixel-for-pixel; 10 points per hit; a
hit at least every 10 seconds or Earth is destroyed; score 1000 (ten ships
of ten hits) saves the Earth; each destroyed ship makes the next change
direction faster. F1/F3/F5 selected difficulty (movement speed 2/3/5);
SHIFT/LOCK toggled crosshair size. Two-player: player 2 flies the alien
ship. The port maps difficulty to 1/2/3 and gives the crosshair shrink to
player 2 as a jam key (F).

## Copyright

Original game design, rules text, and sprite data are from COMPUTE! issue
51, copyright 1984 COMPUTE! Publications, Inc. and the authors. The port's
code is new. Sprite bitmaps in GameData.incl are byte-identical to the
published listing. Do not commit ../devastator-original's program files to a public repo;
they are verbatim copyrighted material.
