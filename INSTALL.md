# BioShock Infinite VR — install & alpha testing

> **This is an alpha.** Read [Known issues](#known-issues) before you install. The world shakes,
> the frame rate swings, and the HUD sits in your peripheral vision. Nothing here is finished.

---

## Requirements

| | |
|---|---|
| **Game** | BioShock Infinite (Steam) |
| **OS** | Windows 10 / 11, 64-bit |
| **Runtime** | Any OpenXR runtime. Developed against **VDXR** (Virtual Desktop) |
| **Headset** | Any OpenXR headset — FOV and per-eye resolution are read from your runtime, not hardcoded |
| **Controllers** | Motion controllers, presented to the game as a gamepad. Mouse + keyboard also work |
| **GPU** | Developed on **AMD**. Untested on NVIDIA — reports welcome |

---

## Install

1. Close the game.
2. Copy **`xinput1_3.dll`** and **`bsi_controls.ini`** into:
   ```
   <Steam>\steamapps\common\BioShock Infinite\Binaries\Win32\
   ```
3. Start your OpenXR runtime — connect Virtual Desktop, or whatever you use — **before** launching.
4. Launch the game normally from Steam.

No game files are replaced. No launch options. No injector.

**To uninstall:** delete `xinput1_3.dll`. The game is back to stock immediately.

### ⚠️ Launch it twice the first time

**Your first launch will render at your monitor's resolution instead of your headset's, and will
look wrong.** This is expected and it fixes itself.

The mod can only learn your headset's per-eye resolution after OpenXR starts — which happens *after*
the game has already created its render target. So the first run measures your headset and writes
`bsi_eye_res.txt` next to the DLL; every launch after that reads it and renders at the right size.

So: **launch, quit, launch again.** From then on it is correct, and you never think about it again.

### Launch order matters

Start the headset runtime **before** the game, every time. If the game starts first the mod logs
`xrGetSystem failed` and falls back to a flat default FOV until a runtime appears.

---

## Controls

Aiming follows your motion controller. Menus take the mouse or the gamepad.

| | |
|---|---|
| Triggers | fire gun / fire vigor |
| Grips | weapon swap / vigor swap |
| A | jump |
| B | crouch and reload |
| X | use / items |
| Y | melee |
| Left stick click | sprint — **hold** it while already moving forward |
| Right stick click | Select |
| Menu | pause |

Everything is remappable in **`bsi_controls.ini`**, next to the DLL. It **reloads about once a
second** — edit, alt-tab, test, no restart. The file documents every option inline, including the two
problems that come up most:

- **A stick does nothing** → it is on an axis this game ignores. Try `Z` / `RZ`.
- **Something steers when it should not** → it is sharing the axis the game uses for look.

### Turning

Snap, smooth, or **off** — all three are first-class choices. Snap makes some people motion sick,
smooth makes others, and some players would rather turn with their body.

### In-headset settings menu — experimental, OFF by default

Create `%TEMP%\bsi_menu_on.txt` to enable it. It works, but it has not had enough headset time to be
trusted on by default, and a settings panel that breaks a game is worse than no settings panel.

**Click both thumbsticks in** to open it. Left stick moves, **A** selects, **B** closes. While it is
open the game receives a neutral gamepad, so navigating does not also walk, shoot or swap weapons.

Settings save to `bsi_vr_settings.ini` beside the DLL.

---

## Known issues

Stated plainly. These are bugs, not "limitations of VR retrofits".

### 🔴 World shake / judder — unresolved

The world shakes, worst on nearby geometry and when you move your head. **This is the top open
defect and it has no fix.** It is intermittent and varies in severity.

Ruled out by measurement, so please don't report these as the cause: frame delivery (zero missed
beats and zero repeated frames through confirmed-bad periods), the stereo separation, the second
render, the pose tagging, or a partially-filled render buffer.

**Before reporting shake, check your desktop is not at 60 Hz.** Windowed presentation is composited
at the *desktop* refresh, and a 60 Hz desktop reintroduces a head-rotation shake that is not the
mod's doing. The mod logs a warning when it detects this.

### 🔴 Frame rate varies by scene

Roughly 37–72 fps at full resolution depending on the area. It is CPU-bound, not GPU-bound.

### 🟡 HUD sits in the far corners

Health and ammo are drawn where a flat game puts them — the screen corners — which in a headset is
deep peripheral vision. Visible, but uncomfortable to look at and easy to miss.

Nothing is cropped: the declared frustum matches the submitted image and its aspect exactly. It is a
placement problem, not a visibility one. **No working fix yet.**

### 🟡 Arms are always visible

Hands-only is not available. The forearm is skinned to the same bone as the hand, so it cannot be
hidden at runtime without removing the hand too. A proper fix needs a re-authored mesh.

### 🟡 One scene pass renders at the wrong size

A second render pass rasterises at a monitor-derived size and is stretched to cover. Cosmetic cover,
not a cure.

### Not implemented

Comfort vignette · motion gestures (reload, melee, grab).

---

## Troubleshooting

**Headset is black, monitor looks fine.**
The runtime was not up when the game launched. Close the game, start the runtime, launch again.

**Everything looks washed out, flat, or wrongly scaled.**
Usually the runtime started after the game, so FOV fell back to a compiled default instead of being
computed from your headset. Relaunch with the runtime already running. If this is your *first ever*
launch, see [Launch it twice](#-launch-it-twice-the-first-time).

**Controllers do nothing, or the wrong stick moves you.**
Edit `bsi_controls.ini` — it reloads live. See the notes inside about which axes this game reads.

**Menus are hard to click.**
The cursor fix is on by default. If it misbehaves, create `%TEMP%\bsi_cursor_scale_off.txt`.

---

## Reporting a bug

The log is at `%TEMP%\BioshockInfiniteVR\mod.log`. It records what the mod actually did — the FOV it
computed, the resolution it asked for, the IPD it read, and every frame-pacing statistic.

⚠️ It rotates per *deploy*, not per *launch*, so one file can hold several sessions. Find the last
`xinput1_3 proxy attached` line and send from there down.

Include:

1. That section of the log
2. Your headset and OpenXR runtime
3. Your GPU (especially if NVIDIA — that is untested)
4. Whether the runtime was running **before** the game started
5. Whether the problem also appears on the monitor, or only in the headset — this one detail
   separates whole classes of cause

---

## Legal

MIT licensed — see `LICENSE`. That covers **this mod's own code and documentation, and nothing
else.** It grants no rights to BioShock Infinite, its engine, or its assets, and **you need your own
legally obtained copy of the game.**

Nothing from the game is redistributed — no assets, no code, no data files. Nothing the game ships is
altered on disk: one extra DLL sits alongside the executable and forwards every real XInput call
through to the system library. Delete that file and the install is exactly as it was.
