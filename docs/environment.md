# Environment and dependencies

## What the project files record

| Item | Value | Source |
|---|---|---|
| Max version saved in the patch | 9.0.8, x64 | `patch/Pulse_of_the_Planets.maxpat`, saved application metadata |
| Platform | Windows | the patch addresses `loopMIDI` virtual ports; the demo recording shows a Windows desktop |
| Synthesizers and mix | Ableton Live | the report; the report's screenshots show Live's MIDI mapping and the devices used |
| Virtual MIDI | loopMIDI ports 1–7 | `loadmess "loopMIDI Port N"` in the patch |
| Ableton Live set | not distributed | see [What is not included](../README.md#what-is-not-included) |
| Demo video | 1920×1080, 60 fps, 662 s | `Pulse_of_the_Planets.mp4` |
| Audio recording | 44.1 kHz stereo, 444 s | `Pulse_of_the_Planets_runAudio.wav` |
| Report | 20 pages | `REPORT.pdf` |

The hardware used to make the work — computer, audio interface, monitoring — was not recorded
in the project files, and the Ableton Live version is not recorded either. The report's Live
screenshots show a FabFilter Pro-Q 4 equaliser and a dearVR MICRO spatialiser in the author's
set; those are commercial plug-ins and are not distributed here.

## What the patch needs

- **Max 9** with Jitter. The patch was saved with 9.0.8; later Max 9 releases should open it.
- Only objects that ship with Max: the note-probability system, the Markov velocity chain
  (`anal`, `prob`), the drum machine (`matrixctrl`, `preset`) and the visuals (`jit.lcd`,
  `jit.window`, `jit.pwindow`) all come with the standard installation. There are no
  third-party externals, abstractions or sample folders.
- **Ableton Live** and a Live set built for it (see the [setup guide](setup-guide.md)).
- **loopMIDI** on Windows, or virtual MIDI ports with the same names.

## Reproduction limits

- The author's Ableton Live set is omitted, so the sound must be rebuilt rather than restored.
  The report documents the intended mapping, and the setup guide lists the tracks and devices
  to recreate.
- The MIDI port names in the patch are Windows-specific. On macOS they must be recreated or
  edited in the patch.
- The visuals assume a 1920×1080 drawing surface; smaller displays will show a scaled or
  clipped Jitter window.
- Media links point to Google Drive. Static repository checks cannot establish that the media
  plays for a signed-out visitor.
- See [verification notes](verification.md) for the checks that were and were not performed.
