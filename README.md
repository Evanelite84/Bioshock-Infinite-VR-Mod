# BioShock Infinite VR Mod

A native VR mod for **BioShock Infinite** — real stereo rendering, 6DoF head tracking, and
motion-controller aim, injected at runtime with no game files modified.

## 📦 Download

The first alpha is available under [Releases](../../releases).

**Read `INSTALL.md` inside the zip before installing.** Install is two files dropped into the game's
`Binaries\Win32` folder, and deleting one of them uninstalls it completely.

⚠️ **Launch the game twice the first time.** The first run renders at your monitor's resolution while
the mod learns your headset's, and corrects itself from the second launch onward.

## 🛠️ Current Status

**Development:** Active, but highly experimental.
**Source Code:** Currently kept in a private repository while core features are being developed.
**Release Timeline:** There is no fixed schedule or release date. It will be ready when it's ready.

### ⚠️ Important: Expect Things to Be Very Broken

This project is **very much a work in progress**. Many things are currently broken, unfinished, or experimental.

The game can also be **fairly unstable and shaky in VR** at the moment. Things like **FOV, resolution, camera settings, and other visual parameters may not be correct out of the box** and may require you to make your own adjustments to get things looking or feeling right for your setup.

There is now a **basic in-headset settings menu**, but it is **experimental and disabled by default** — see `INSTALL.md` if you want to turn it on. Turning style (smooth, snap, or off) and hiding the crosshair are the settings that have had real headset testing. Most other adjustments still have to be made by hand.

I would **not recommend downloading this unless you are comfortable tinkering, changing settings, and troubleshooting things yourself**. There is currently no technical support or troubleshooting available.

Development and testing happen almost entirely on **AMD hardware**. I have not been able to fully test on NVIDIA, but I have confirmed that the mod **boots and runs without crashing** on an NVIDIA GPU. How well it actually performs there is still an open question, so treat NVIDIA as **not fully tested** rather than unsupported.

In short: **expect bugs, broken features, crashes, visual issues, unstable behavior, and general jank.** Getting the mod working properly may require some experimentation and manual adjustments on your end.

### What actually works

- Real stereo rendering — both eyes from one frame at one head pose, not a reprojection
- 6DoF head tracking, including leaning
- Motion-controller aim
- FOV and per-eye resolution read from **your** headset, not hardcoded or tied to your monitor
- Depth submitted to the compositor
- Smooth, snap, or **no** turning — all three are proper options
- Crosshair can be hidden

### The big open one

**The world shakes**, worst on nearby geometry and when you move your head. It has no fix yet and it
is the first thing you will notice. Frame rate also swings roughly 37–72 fps depending on the area,
and the HUD sits in the far corners of your vision. All of this is written up honestly in
`INSTALL.md` rather than buried.

## 📢 Communication

This is a solo hobby project, and my priority is spending my limited time actually working on the mod.

Because of that, I will not be responding to:

* General questions or inquiries
* Feature requests
* Support requests
* Troubleshooting questions

Please keep this in mind before reaching out.

## 🔮 The Future of This Project

Another developer has recently released their own VR mod for **BioShock Infinite**. Given that I am still relatively new to VR modding, I believe their project may ultimately be more complete and polished than mine.

As a result, **I may not end up finishing this project**. If another solution becomes the better option for the community, I have no problem with that.

For now, I'm experimenting, learning, and seeing how far I can take this.

**No promises, no guarantees, and definitely no ETA. Just a work in progress.**
