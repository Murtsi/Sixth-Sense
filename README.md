# Sixth Sense

A first-person obstacle-awareness concept for people with visual impairments.

Junction 2026: **AI Glasses for Everyone, Everyday by Meta**.

**Status: documentation and architecture only. No working application, detector, device integration or measured results exist in this repository.**

The proposed audio-first experience uses the glasses' view to notice selected obstacles, particularly at head height, and give a short spoken or optional haptic cue. The wearable benefit is hands-free sensing from near eye level while the user's hands remain available for their usual mobility aid.

We will validate this need with people with visual impairments. The prototype is not a replacement for a cane, guide dog or mobility training, and it must not claim that a route is safe. Device access and latency are unverified. Replay and phone-camera experiments must be labelled as such, not presented as a live glasses demonstration.

## Project documents

| Document | Purpose |
|---|---|
| [CHALLENGE.md](CHALLENGE.md) | Supplied Meta requirements, resources and judging weights |
| [IDEA.md](IDEA.md) | Audience, focused use case and scope |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Proposed device/perception/policy flow and failure states |
| [TEAM.md](TEAM.md) | A/B/C responsibilities and M1–M7 contracts |
| [TODO.md](TODO.md) | Ordered tasks, dependencies and go/no-go questions |
| [docs/DECISIONS.md](docs/DECISIONS.md) | Decisions and assumptions |
| [docs/DEMO.md](docs/DEMO.md) | Controlled demo and fallback plan |
| [docs/JUDGING.md](docs/JUDGING.md) | Evidence needed for each criterion |
| [docs/PITCH.md](docs/PITCH.md) | Deck outline |
| [docs/SUBMISSION.md](docs/SUBMISSION.md) | Submission checklist |
| [SECURITY.md](SECURITY.md) | Private data and disclosure guidance |
| [AGENTS.md](AGENTS.md) | Contributor rules |

## First decisions

Confirm the camera stream, output and device-state capabilities with Meta before selecting mobile platform, SDK or model. Audio-first is the working direction, not an excuse to assume that Bluetooth audio, stereo, gestures or background access are available.

The planned architecture processes awareness signals locally; optional cloud description is a separate, consented feature. A working offline path, latency and battery performance must be demonstrated before claiming them.

The task board includes replay development so teammates can work while devices are shared. The final evidence must distinguish replay, phone camera and actual glasses operation. No executable scaffold or setup commands are included. No license has been selected.

Source: the supplied challenge description, summarised in [CHALLENGE.md](CHALLENGE.md). Existing detailed planning documents are partly in Finnish.
