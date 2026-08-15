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
| **GPU** | Developed on **AMD**. Confirmed to boot and run on NVIDIA, but not fully tested there |

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

### If your runtime starts late, that's fine

Starting the headset runtime **before** the game is still the recommended order. But if you launch
first and connect afterwards, the mod now **waits up to 5 minutes for a headset to appear** and picks
it up automatically — no relaunch needed. You'll see it in the log:

```
openxr: no headset available yet -- waiting. Attempt 1; connect your VR runtime and it will pick up automatically.
openxr: headset appeared after 8 retries (~16 s of waiting) -- continuing init
```

If no headset ever shows up, it gives up after about five minutes and the game just carries on flat.

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

Already ruled out by measurement: frame delivery (zero missed beats and zero repeated frames through
confirmed-bad periods), the stereo separation, the second render, the pose tagging, and a
partially-filled render buffer.

What it *is*, measured: the compositor has to warp each frame to match where your head has moved to
by the time it reaches your eyes, and **the amount of that correction varies frame to frame**. Near
geometry gets displaced differently every frame, which is why it is worst on the gun and close walls.
The correction scales with render latency, so it is much smaller at lower resolution — a trade-off
with a knob, not a mystery, but not solved either.

**The one thing on your end that makes it worse:** a machine that cannot keep up. Look for
`pacing: beats` in the log — a high `missed` count means frames are not arriving in time, and that
reads as shake regardless of anything else. Lowering your headset's render resolution in your
runtime's settings is the lever if so.

> **Ignore the mod's own "DESKTOP IS ONLY nn Hz" warning.** It is stale. The mod presents unsynced
> from the desktop refresh and takes its timing from the headset instead, so your desktop refresh
> rate does not affect what you see in the headset — only the flat mirror on your monitor, which can
> judder while the headset is fine. Verified at 60 Hz with 353 of 356 frames on time.

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
Connect your runtime — the mod waits up to 5 minutes for a headset and will pick it up without a
relaunch. If it has been longer than that, quit and start again with the runtime already running.

**Everything looks washed out, flat, or wrongly scaled.**
If this is your *first ever* launch, that is expected — see
[Launch it twice](#-launch-it-twice-the-first-time). Otherwise check the log for `widefov:`; if it
says *compiled default* rather than *computed from the headset's own FOV*, the mod never got a
headset and the flat fallback is what you are looking at.

**Controllers do nothing, or the wrong stick moves you.**
Edit `bsi_controls.ini` — it reloads live. See the notes inside about which axes this game reads.

**Menus are hard to click.**
The cursor fix is on by default. If it misbehaves, create `%TEMP%\bsi_cursor_scale_off.txt`.

### The log tells you what actually happened

`%TEMP%\BioshockInfiniteVR\mod.log` records what the mod really did — the FOV it computed, the
resolution it asked for, the IPD it read from your headset, and every frame-pacing statistic. If
something looks wrong, this is the first place to look rather than guessing.

Useful lines to search for:

| search for | tells you |
|---|---|
| `swapsize:` | what resolution it rendered at, and where that number came from |
| `widefov:` | the FOV it computed from your headset, or the default it fell back to |
| `viewconfig:` | what your runtime actually recommended |
| `sanity:` | your desktop resolution and refresh — a low refresh reintroduces shake |
| `pacing: beats` | whether your machine is keeping up. Lots of `missed` means it is not |

⚠️ The log rotates per *install*, not per *launch*, so one file can hold several sessions. Find the
last `xinput1_3 proxy attached` line and read from there down.

---

## Bug reports

**Not being taken right now, and issues are disabled on the repository.** This is a solo hobby project
and the time goes into the mod rather than into a support queue. Nothing here is abandoned — it just
isn't a supported release, and pretending otherwise would waste your time as much as mine.

The known problems are listed above, honestly and in full. If you hit one of those, it is known.

---

## Legal

MIT licensed — see `LICENSE`. That covers **this mod's own code and documentation, and nothing
else.** It grants no rights to BioShock Infinite, its engine, or its assets, and **you need your own
legally obtained copy of the game.**

Nothing from the game is redistributed — no assets, no code, no data files. Nothing the game ships is
altered on disk: one extra DLL sits alongside the executable and forwards every real XInput call
through to the system library. Delete that file and the install is exactly as it was.
