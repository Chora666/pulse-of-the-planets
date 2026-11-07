# Release notes

## v1.0.0 — initial public release

First publication of *Pulse of the Planets*.

### Included

- `patch/Pulse_of_the_Planets.maxpat` — the complete Max/MSP patch, one self-contained file:
  movement, distance maths, note-probability and pitch-quantisation systems, Markov velocity
  humanisation, MIDI out, drum machine, arrangement and Jitter visuals.
- `REPORT.pdf` — the written report (20 pages), with patch screenshots for every subsystem.
- `docs/` — operating guide, AI-assistant setup guide, environment notes, verification notes
  and patch diagrams.
- Demo video and audio recording, hosted on Google Drive and linked from the README.

### Known limitations

- The author's Ableton Live set — instruments, racks and MIDI mapping — is not part of the
  repository. The patch sends MIDI; the sound must be rebuilt in Live following the report and
  the [setup guide](setup-guide.md).
- The patch addresses Windows `loopMIDI` port names (`loopMIDI Port 1`–`7`). On macOS those
  names must be recreated or edited in the patch.
- No automated tests; verification is documented in [verification.md](verification.md).

### Licence

Patch: PolyForm Noncommercial 1.0.0. Documentation and report: CC BY-NC 4.0. See
[NOTICE.md](../NOTICE.md).
