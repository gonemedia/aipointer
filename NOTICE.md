# NOTICE — License Change

**Effective 2026-05-19, AIPointer switches from the MIT License to the Business Source License 1.1 (BSL-1.1).**

## What changed

- Previous license: MIT (releases up to and including v1.1.1).
- New license: BSL-1.1 (v1.1.2 onwards).
- Licensor: Mario Simic.
- Change Date: **2030-05-19** — on that date, AIPointer automatically converts to the **Apache License, Version 2.0**.
- Older releases stay under MIT. Tags and binaries published before 2026-05-19 are unaffected.

## Why

AIPointer is a side-product of [Skales](https://skales.app) — Mario's main project, which is itself BSL-1.1. Tools, ideas, and code routinely flow between the two repositories. Running both projects under different licenses created friction: contributions and patterns invented in one would have had to be re-licensed before they could land in the other.

Aligning AIPointer with Skales fixes that. It also draws a clear line between two kinds of use:

- **Free, no questions asked:** personal, educational, internal business use, and source inspection.
- **Requires a commercial license:** SaaS hosting, white-labeling, resale, and bundling AIPointer into a commercial product distributed to third parties.

That line is what BSL-1.1 was designed to draw. It is the same model used by Sentry, MariaDB MaxScale, CockroachDB, and HashiCorp Terraform.

## What stays the same

- The source remains public on GitHub. Anyone can read it, fork it, build it, audit it.
- Personal use, learning, classroom use, and internal company use stay free.
- No telemetry. No cloud. Bring-your-own API key.
- On 2030-05-19, the whole codebase reverts to Apache 2.0. The BSL clauses are time-limited by design.

## What's new

- `LICENSE` — the full BSL-1.1 text with the parameters filled in.
- `COMMERCIAL-LICENSE.md` — short note on when a commercial license is required.

---

Commercial licensing: **dev@mariosimic.at**

---

## Third-party model attributions (v1.1.5+)

AIPointer's optional **Local voice engine** uses models and runtimes that the user explicitly downloads after opting in. These are NOT bundled in the base installer; AIPointer fetches them from Hugging Face Hub on first activation.

### Kokoro 82M (Text-to-Speech)

- **Author:** hexgrad
- **Project:** https://huggingface.co/hexgrad/Kokoro-82M (ONNX export: https://huggingface.co/onnx-community/Kokoro-82M-ONNX)
- **License:** Apache License 2.0
- **Inference library:** `kokoro-js` by Xenova / Hugging Face — Apache 2.0

Use in AIPointer's Local TTS engine is permitted under Apache 2.0. The model weights are downloaded to the user's local app data folder and are not redistributed by AIPointer's installer.

### whisper.cpp / Whisper GGML models (Speech-to-Text)

- **whisper.cpp** by Georgi Gerganov: https://github.com/ggml-org/whisper.cpp — MIT License
- **GGML whisper-base.bin** mirror: https://huggingface.co/ggerganov/whisper.cpp — MIT License (model weights derived from OpenAI Whisper, MIT)
- **Node binding:** `@kutalia/whisper-node-addon` — MIT License (prebuilt binaries for Win x64, Linux x64/arm64, macOS x64/arm64)
- **OpenAI Whisper** (original model architecture and training): https://github.com/openai/whisper — MIT License

Use in AIPointer's Local STT engine is permitted under MIT. The model weights are downloaded to the user's local app data folder and are not redistributed by AIPointer's installer.
