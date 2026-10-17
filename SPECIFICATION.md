# MXL LightScore Format — Experimental Draft 0.1

Status: **experimental, not a stable interoperability standard**.

MXL is a hardware-independent description of timed control cues synchronized to a media timeline. Its purpose is to interchange creative intent between authoring software and compatible renderers, including lighting, display, fountain, and kinetic systems.

## Representation

Draft 0.1 uses UTF-8 JSON with the extension `.mxl`. Media is referenced rather than embedded. All times are integer microseconds from the media timeline origin, avoiding floating-point drift. A renderer MUST reject unsupported major versions and MUST NOT interpret unknown track types as hardware commands.

## Document

Required top-level fields: `mxlVersion`, `title`, `durationUs`, `tracks`. Optional `media` holds a URI and optional SHA-256 digest. Each track has a unique `id`, a `type`, and time-ordered `cues`. Each cue has `atUs` and an abstract `values` object. A cue may specify `durationUs` and `interpolation` (`step` or `linear`).

## Draft track types

- `light.color`: normalized red/green/blue channels from 0 to 1, optional brightness from 0 to 1.
- `light.bar`: abstract fixture intensity and segment patterns; physical mapping is external.
- `display.color`: synchronized colour for audience screens or displays.
- `fountain.jet`: normalized requested jet height from 0 to 1; requires a validated equipment adapter.
- `motion.axis`: normalized position or velocity intent; requires a validated equipment adapter.

Track type definitions are preliminary; unknown types must be preserved by editors when feasible but ignored by playback engines unless explicitly supported.

## Synchronization

Cues are positioned on the media timeline, not relative to receipt of a network packet. Implementations should synchronize clocks, schedule future cues, and report observed output latency. Clock agreement alone does not guarantee optical or mechanical synchronization.

## Safety

MXL files are **not** executable commands. Equipment adapters MUST enforce device-specific limits, interlocks, emergency-stop behavior, and operator authorization. Never send unvalidated MXL cue values directly to motors, pumps, lasers, pyrotechnics, or other hazardous hardware.

## Compatibility and governance

This is an initial proposal intended for public discussion. No interoperability, patent, trademark, or safety certification is implied. Changes to this draft may be breaking. See the repository issues for proposals and feedback.
