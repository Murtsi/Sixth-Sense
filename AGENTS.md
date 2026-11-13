# Contributor and agent instructions

This is a documentation-only Sixth Sense concept for Meta's AI Glasses challenge. Read CHALLENGE.md, IDEA.md, ARCHITECTURE.md, TEAM.md, TODO.md and CHANGELOG.md before work.

- Do not claim a working app, device capability, model, benchmark, user study or pilot without evidence.
- Determine lane A/B/C from the branch and TEAM.md. Agree M1–M7 changes with affected teammates; do not silently change another lane's interface.
- Use feature branches and PRs. Record meaningful changes in CHANGELOG.md and decisions in docs/DECISIONS.md. Keep task status honest. No force-pushes to shared branches.
- Check actual DAT/web/mobile capabilities with Meta before choosing an SDK/platform. Audio-first and local inference are intended design directions, not verified implementation facts.
- Keep credentials, identifying images/voices, consent forms and restricted resources outside public Git. Use synthetic/redacted examples.
- Clearly label replay, phone-camera and actual glasses paths. Never represent a latency target as a measurement.
- This prototype does not establish a safe route. Do not stage blindfold obstacle navigation, traffic or stair tests that rely on it. Use the controlled demonstration in docs/DEMO.md.
