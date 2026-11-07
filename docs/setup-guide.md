# Setup and operating guide

How to open, connect, play and record the patch. Software requirements are listed in the
[main README](../README.md); this page is the operating manual.

## 1. Project files

| File | Purpose |
|---|---|
| `patch/Pulse_of_the_Planets.maxpat` | The complete Max/MSP patch — movement, distance maths, note and pitch probabilities, automation, drum machine, arrangement and Jitter visuals |
| `REPORT.pdf` | The written report, with patch screenshots for every subsystem |

The patch is a single file with no abstractions, no sample folders and no external files.
Nothing needs to sit next to it. Open it directly in Max.

## 2. Software

- **Max 9** with **Jitter**. The patches were saved with Max **9.0.8** (x64). The visuals use
  `jit.lcd`, `jit.window` and `jit.pwindow`; Jitter must be available.
- **Ableton Live** as the sound source and mixer.
- **loopMIDI** (Windows) or an equivalent virtual MIDI setup.

The patch uses `anal` and `prob` for the velocity Markov chain. Both ship with Max, so no
package installation is required.

## 3. Virtual MIDI

Max sends notes with `noteout` and continuous controllers with `ctlout`. The patch's
`loadmess` objects set the MIDI destinations by name, and those names are:

```
loopMIDI Port 1
loopMIDI Port 2
loopMIDI Port 3
loopMIDI Port 4
loopMIDI Port 5
loopMIDI Port 6
loopMIDI Port 7
```

**Windows.** Install [loopMIDI](https://www.tobias-erichsen.de/software/loopmidi.html) and
create seven ports with exactly those names. Start loopMIDI before opening the patch. Live
will then show seven MIDI inputs.

**macOS.** The names above do not exist on macOS, and the patch does not fall back to another
port. Two options:

1. Use a virtual MIDI tool that lets you create ports with arbitrary names, and create the
   seven names above.
2. Edit the `loadmess "loopMIDI Port N"` messages in the patch to the name of an IAC bus
   (`Audio MIDI Setup → Window → Show MIDI Studio → IAC Driver → Device is online`, then use
   e.g. `IAC Driver Bus 1`). The patch is plain JSON, so a text editor works; keep a backup.

**What arrives.** The report's MIDI-out screenshot shows the note streams and the controller
streams leaving on their own channels, and the Live mapping screenshot shows the author's
assignments on channels 1–3. Notes arrive on `noteout` channels 1–8; controllers arrive with
`ctlout` on the channels and controller numbers shown in the report's *Filter automation*,
*Volume automation* and *Pan automation* sections. Map them in Live to match your own set.

## 4. The Ableton Live set

The author's Live set is **not** part of this repository (see
[What is not included](../README.md#what-is-not-included)). Rebuild it once, then leave it
open while the patch runs:

1. Create a MIDI track for each pitched layer — one for synth A, one for synth B and one bass
   track — and set each track's MIDI input to the virtual port that carries its channel.
2. Create a drum track with a Drum Rack on the channel that carries the drum notes.
3. Map the controller streams. The author's Live set used:
   - a band gain on a **FabFilter Pro-Q 4** equaliser as the shelving filter that follows
     distance from screen centre,
   - the track's **Mixer → Track Volume** for the distance-driven level,
   - **dearVR MICRO → Azimuth** for the horizontal-position pan,
   - a **limiter input gain** stage on the pads.
   The mapping screenshot in the report (page 9) lists the exact CC numbers, channels,
   devices and ranges.
4. Save the set. The patch does not depend on any preset name.

## 5. First run

1. Start loopMIDI (Windows) or your virtual MIDI device.
2. Open Ableton Live with the set from §4 and arm the tracks or enable monitoring.
3. Open `patch/Pulse_of_the_Planets.maxpat` in Max.
4. The patch starts on its own: a `loadbang` sends the start message, a Jitter window opens
   and MIDI begins to flow. If the presentation window is not visible, switch Max to
   presentation mode.
5. Check that the three planets move and that Live receives notes. If the planets do not move,
   click the blue toggle in the upper right of the presentation (it enables the timeline and
   visual layer), or reload the patch.
6. Press the space bar or click **Click or Space** to interact.

## 6. Interface

- **Operation Time** — the running counter for the current performance.
- **Pulse of the Planets** — the title strip.
- **The visual field** — the black frame with the three bodies. Halo, line and Boom events draw
  over it; the same visuals can be shown full-screen in the Jitter window.
- **Gravitational / the text column** — the status label and the poetic instructions on the
  right (*The universe listens to your touch*, *A soft tap whispers to the stars*, *repeated
  taps quicken their pulse*, *Or perhaps*, *Do nothing at all*).
- **Click or Space** — the manual trigger.
- **Blue toggle** — enables the timeline and star-system layer (see §5).

## 7. Interaction

- One click or space press pulls the global ratio down, which speeds everything up. The value
  starts to rebound after the gesture; pressing again within 0.8 seconds delays the rebound
  until the clicking stops.
- **15 presses within 4 seconds** triggers *Crazy Moment*: all global parameters multiply and
  the piece goes chaotic for a moment.
- Any interaction can force the drop, whether or not *Crazy Moment* fires.
- Stop interacting and the ratio decays: the piece falls back into the breakdown.

## 8. Arrangement

The intended listening sequence, and the one heard in the demo, is:

`Intro – Verse – Highlight Moment (manual) – Drop – Verse – Highlight Moment – Drop – Verse –
Ending`

Left alone, the patch moves from the intro into the verse on the gold body's probabilistic
triggers, and the verse evolves on its own. The first three successful triggers accelerate the
ratio; the verse then keeps changing inside a bounded speed range. The drop is what the
listener triggers.

## 9. Recording

The patch has no recorder; it sends MIDI only. Record Live's stereo output with Live's own
recorder (an audio track in resampling mode or the export command), or capture the computer's
audio output externally. The audio file linked from the README is a 7 min 24 s stereo run at
44.1 kHz.

## 10. Verify a fresh setup

Use the [verification checklist](verification.md) to check the patch, the MIDI ports, the Live
set and the visuals on a new installation. The demo recording documents the system running in
performance; no new runtime test was performed for this documentation update.

## 11. Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Patch opens but no MIDI reaches Live | Virtual ports missing or named differently | Create `loopMIDI Port 1`–`7` exactly; restart Live after creating them; on macOS edit the port names in the patch |
| Planets do not move / no visuals | Timeline toggle off, or Jitter unavailable | Click the blue toggle; confirm Jitter is installed with Max |
| Notes arrive on the wrong track | Track MIDI input channel or source port does not match | Check the port name in Max and the channel in Live's track input |
| Sound is silent | Live track not armed / no instrument loaded | Load an instrument, arm or monitor the track |
| Filter, level or pan do not move | CCs not mapped in Live | Re-map the controllers using the report's mapping screenshot |
| Everything accelerates and never settles | A stuck key or mouse press | Stop pressing; the ratio decays on its own. If not, reload the patch |
