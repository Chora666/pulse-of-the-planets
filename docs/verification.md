# Verification notes

## What was checked

Static checks were run on the repository during the documentation pass (7 November 2026):

| Check | Result |
|---|---|
| `patch/Pulse_of_the_Planets.maxpat` parses as JSON | Pass |
| Patch has no dependency cache and references no external files | Pass — single self-contained file |
| MIDI destinations referenced by the patch | `loopMIDI Port 1` through `loopMIDI Port 7` |
| Objects that require third-party external libraries | None found; `anal`, `prob`, `matrixctrl`, `preset` and Jitter ship with Max |
| `REPORT.pdf` opens and has the expected page count | Pass — 20 pages |
| Repository images referenced by both README files exist | Pass |
| Media URLs | Hosted on Google Drive; set to *anyone with the link can view* |

The patch's saved application metadata records Max 9.0.8 (x64) and its MIDI port names record a
Windows `loopMIDI` setup. The demo recording shows the system running in performance and is the
evidence for the operation described in the documentation. **No new runtime test was performed
on the machine that prepared this repository**, and the author's Ableton Live set was not
available for testing: the checks above cover the files, not a fresh end-to-end performance.

## Optional checks on a fresh installation

Run these yourself when preparing a new machine. They are listed in the order that isolates
failures fastest.

1. **Patch opens.** Open `patch/Pulse_of_the_Planets.maxpat` in Max with Jitter installed. No
   missing-file dialogs should appear.
2. **Visuals run.** The three bodies move inside the black frame; the halo, line and Boom
   events appear as notes fire.
3. **MIDI ports exist.** loopMIDI shows `loopMIDI Port 1`–`7`; Live lists them as inputs.
4. **Notes arrive.** With a Live set loaded, an instrument on the matching channel produces
   sound and Live's MIDI indicators move.
5. **Automation arrives.** Moving a body toward the screen centre brightens the mapped
   filter; the overall level and pan follow the reported mapping.
6. **Interaction works.** A space press or click accelerates the motion; 15 presses within
   four seconds produces the *Crazy Moment* behaviour; a drop follows.
7. **Form advances.** Left alone, the piece moves from intro into verse; the drum machine
   engages as the global ratio crosses its threshold.

Report the results of steps 4–7 only from the human's observation: they involve hearing and
seeing a GUI application, which cannot be checked from the repository.
