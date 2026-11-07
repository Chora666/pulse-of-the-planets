# Setup guide for AI assistants

*Written for AI assistants helping a human install and run this project. If you are a person,
[the setup guide](setup-guide.md) is the shorter route.*

You are reading this because someone asked you, an AI agent, to help run **Pulse of the
Planets** — an audiovisual interactive work in Max/MSP that generates real-time ambient music
and sends it as MIDI to Ableton Live.

This file tells you what is in the repository, what is missing, what to install, how to connect
MIDI, and how to verify the result. Follow it in order.

**Read this first, it changes the plan:** Max and Ableton Live are GUI applications. You cannot
launch them headlessly or hear the output. Your job is to prepare the environment, run the
static checks below, and then guide the human through a handful of GUI steps and ask them to
confirm what they see. Do not claim the audio works — only the human can hear it.

---

## 1. Facts, verified from the files

| Item | Value |
|---|---|
| Repository | `https://github.com/Chora666/pulse-of-the-planets` |
| Patch | `patch/Pulse_of_the_Planets.maxpat` — one file, no abstractions, no samples |
| Max version saved in the patch | 9.0.8 (x64) |
| Developed on | Windows — the patch addresses `loopMIDI Port 1`–`7` |
| Virtual MIDI | loopMIDI on Windows; see §4 for macOS |
| Ableton Live set | **Not included.** The author's instruments, racks and MIDI mapping are not in the repository |
| Media | demo video and audio recording are on Google Drive, not in the repo |
| Written report | `REPORT.pdf`, 20 pages |

The patch is self-contained. It uses only objects that ship with Max, including `anal`, `prob`
and Jitter.

## 2. Clone and inspect

```bash
git clone https://github.com/Chora666/pulse-of-the-planets.git
cd pulse-of-the-planets
```

Validate the patch and count objects without opening Max:

```bash
python3 - <<'PY'
import json
d = json.load(open("patch/Pulse_of_the_Planets.maxpat"))
p = d["patcher"]
def count(pat):
    n = len(pat.get("boxes", []))
    for b in pat.get("boxes", []):
        if "patcher" in b.get("box", {}):
            n += count(b["box"]["patcher"])
    return n
print("maxpatcher version:", p["appversion"])
print("objects:", count(p))
print("loadmess ports:", sorted({b["box"]["text"] for b in p["boxes"]
      if "loadmess" in (b.get("box", {}).get("text") or "")
      and "loopMIDI" in b["box"]["text"]}))
PY
```

Expected: app version 9.0.8, roughly 2,500 objects in total, and `loadmess "loopMIDI Port 1"`
through `loadmess "loopMIDI Port 7"`.

Then check the report opens:

```bash
python3 -c "import pypdf; print(len(pypdf.PdfReader('REPORT.pdf').pages))"
```

20 pages. Install `pypdf` if needed.

## 3. What the patch does

One patcher holds the whole work:

- **Movement** — three bodies (`A`, `B`, `C`) each pick random targets and travel toward
  them; their centres are published on send/receive buses (`Xc1`…`Yc3`).
- **Distance** — pairwise distances via the Pythagorean theorem (`Distance1-2` etc.).
- **Note probability** — distance is clipped, power-shaped and mapped to a trigger probability
  checked against `random 1000`.
- **Pitch** — index into a Dorian-mode `coll` (`DorianMode`), a modal-stability probability
  layer, itables for continuous quantisation, and octave offsets for the two synths and the
  bass.
- **MIDI out** — `noteout` channels 1–8 and `ctlout` for filter/level/pan automation.
- **Drums** — `matrixctrl` + `preset` for a four-bar, 16-step sequence, gated by the global
  ratio.
- **Form** — timeline, ratio change, verse and drop systems, and the `CrazyMoment` subpatcher.
- **Visuals** — `jit.lcd` drawing at 1920×1080, with `HaloSystem`, `Boom`,
  `CloseLineSystem` and `StarSystem` subpatchers.

## 4. Connect MIDI

The patch sets its MIDI destinations by name at load time:

```
loopMIDI Port 1 … loopMIDI Port 7
```

**Windows:** install loopMIDI, create those seven ports, start loopMIDI before opening Max,
then start Live.

**macOS:** those port names do not exist. Either create virtual ports with those names, or edit
the `loadmess "loopMIDI Port N"` messages to your IAC bus name. Do not silently skip this: the
patch does not fall back to another port.

## 5. The Ableton Live set

It is **not** in the repository, and there is no `.als` file to open. Tell the human plainly.
To run the sound they must build a Live set:

1. Two pitched tracks (synths A and B) and a bass track, each listening to the virtual port and
   the MIDI channel the patch sends on.
2. A drum track with a Drum Rack for the drum channel.
3. Continuous controllers mapped to filter band gain, track volume and pan/azimuth. The report
   page 9 shows the author's mapping: a FabFilter Pro-Q 4 band gain, track volume, a limiter
   input gain and dearVR MICRO azimuth, on CC1–CC7 across channels 1–3.

If the human has no Live set, they can still open the patch and watch the visuals; there will
be no sound.

## 6. Launch and verify

1. Virtual MIDI running (§4).
2. Live open with the set from §5.
3. Open `patch/Pulse_of_the_Planets.maxpat` in Max.
4. The patch starts itself from a `loadbang`: a Jitter window opens and MIDI starts.

What to ask the human:

| Ask | Expected |
|---|---|
| "Did Max open without missing-file dialogs?" | Yes — the patch needs no external files |
| "Are the three bodies moving in the black frame?" | Yes |
| "Does Live receive MIDI notes on the mapped tracks?" | Yes, meters or instrument activity |
| "Can you hear the piece?" | Yes, once the Live instruments and mapping exist |
| "Does pressing the space bar change the motion?" | Yes, the global ratio drops and the texture speeds up |

## 7. When it does not work

| Symptom | Cause | Fix |
|---|---|---|
| No MIDI in Live | Virtual ports missing or renamed | Create `loopMIDI Port 1`–`7`; restart Live; on macOS edit the port names in the patch |
| No visuals | Jitter missing, or the blue toggle is off | Install Jitter with Max; toggle the blue control in the presentation |
| Patch appears static | Max not running the scheduler (audio off is fine, but transport must run) | Reload the patch; check that the timeline toggle is on |
| Wrong track gets notes | Channel/port mismatch | Compare the `noteout` channel with the track's input channel |
| No filter/volume/pan movement | CCs not mapped | Rebuild the mapping from report page 9 |

## 8. Your boundaries

- Do not claim to have heard the piece. Static checks and the human's report are all you have.
- Do not invent an Ableton Live set, instrument presets or CC mappings beyond what the report
  documents; if in doubt, ask the human.
- Do not edit the report PDF or the licence notices. The patch is licensed for noncommercial use
  only; a separate licence is required for commercial use (see `NOTICE.md`).
- Do not upload the authors' media anywhere. Video and audio live on Google Drive; link to them,
  do not copy them.

## 9. Further reading

- [`setup-guide.md`](setup-guide.md) — the operating guide: ports, Live set, interface,
  interaction, arrangement, recording.
- [`../REPORT.pdf`](../REPORT.pdf) — the full system description with patch screenshots.
- [`architecture/`](architecture/) — the diagrams used in the README.
- [`environment.md`](environment.md) — what the project files do and do not record.
