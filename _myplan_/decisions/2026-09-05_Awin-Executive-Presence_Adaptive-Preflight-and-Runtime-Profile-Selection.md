# Decision — Awin Executive Presence Adaptive Preflight and Runtime Profile Selection

**Date:** 2026-09-05  
**Status:** Accepted planning decision  
**Decision owner:** Chairman  
**Planning source:** Chairman + ChatGPT  
**OpenSpec ownership:** TBD by Antigravity after repository inspection

## Decision

Before installing or starting Awin Executive Presence, the system must run an automated **Preflight Capability Check** and resolve a compatible runtime profile.

The installer must not blindly install the highest-quality/default stack and fail later. It must first inspect the host, determine what the machine can actually support, then choose the best validated combination of packages/models for that environment.

Canonical flow:

```text
Host machine
   ↓
Capability Scanner
   ↓
Capability Report
   ↓
Runtime Profile Resolver
   ↓
Compatibility / Resource Rules
   ↓
Install Plan
   ↓
Chairman-visible recommendation
   ↓
Install / Start
   ↓
Post-install smoke test + health report
```

## What the Preflight Must Inspect

At minimum:

```yaml
hardware:
  os:
  cpu_arch:
  cpu_cores:
  ram_total_gb:
  disk_free_gb:
  gpu_vendor:
  gpu_model:
  gpu_count:
  vram_total_gb:
  vram_free_gb:

runtime:
  windows_version:
  wsl_present:
  wsl_distribution:
  wsl_version:
  nvidia_driver:
  cuda_runtime:
  cuda_visible_in_wsl:
  python_versions:
  uv_present:
  docker_present:
  ffmpeg_present:
  git_present:

network_media:
  localhost_access:
  required_ports_available:
  hyperv_excluded_ports:
  webrtc_capability:
  stun_reachability_when_required:
  microphone_secure_context:

software_models:
  llama_cpp_available:
  stt_provider_available:
  tts_provider_available:
  avatar_provider_available:
  existing_model_artifacts:
  model_checksums_valid:
  private_voice_profile_present:
  avatar_profile_present:
```

## Resolver Principle

The resolver selects the **best compatible validated profile**, not simply the lowest common denominator.

Priority:

```text
1. correctness / compatibility
2. reliability
3. realtime performance
4. privacy / local execution preference
5. quality
6. resource efficiency
```

A faster or newer package must not be selected if it breaks compatibility with the rest of the validated stack.

## Example Adaptive Profiles

### Profile HQ — Dedicated / High VRAM

```yaml
requirements:
  vram_gb: ">= 32 or dedicated avatar GPU"

stack:
  llm: larger_local_quantized_model
  stt: faster-whisper-large-v3
  tts: qwen3-tts
  avatar: livetalking
  lip_sync: musetalk_1_5
```

### Profile Balanced — 24 GB Single GPU

```yaml
requirements:
  vram_gb: ">= 20"

stack:
  llm: qwen_14b_or_equivalent_quantized_if_headroom_allows
  stt: faster-whisper-medium_or_large_v3
  tts: qwen3-tts
  avatar: livetalking
  lip_sync: wav2lip
```

### Profile Compact — 12–16 GB GPU

```yaml
stack:
  llm: qwen_8b_quantized_or_equivalent
  stt: faster-whisper-medium
  tts: qwen3-tts_or_lower_memory_validated_tts
  avatar: livetalking
  lip_sync: wav2lip
  llm_context: reduced
```

### Profile Minimal — ~8 GB GPU

```yaml
stack:
  llm: qwen_4b_quantized_or_equivalent
  stt: faster-whisper-small_or_medium
  tts: validated_low_memory_tts
  avatar: wav2lip_if_measured_safe
  fallback: text_only_or_voice_only
```

### Profile Hybrid

When local hardware cannot meet acceptable realtime requirements:

```yaml
local:
  - microphone
  - stt_optional
  - tts_optional
  - avatar_optional

remote:
  - approved_llm_provider
  - or approved_gpu_worker
```

The resolver may choose hybrid execution instead of forcing a poor local experience.

## Package Fallback Rules

The system should maintain a validated compatibility matrix instead of ad-hoc substitutions.

Example:

```yaml
capabilities:
  stt:
    preferred:
      provider: faster-whisper
      model: large-v3
    fallbacks:
      - faster-whisper-medium
      - faster-whisper-small

  tts:
    preferred:
      provider: qwen3-tts
    fallbacks:
      - cosyvoice3
      - other_validated_low_memory_provider

  avatar_lipsync:
    preferred_high_quality: musetalk_1_5
    preferred_balanced: wav2lip
    fallback:
      - voice_only
      - text_only

  llm:
    preferred_local: larger_quantized_model
    fallbacks:
      - qwen_8b_quantized
      - qwen_4b_quantized
      - approved_remote_provider
```

Fallbacks must be ordered, tested and version-pinned. The installer must not download arbitrary replacements from search results at runtime.

## Compatibility Matrix

Antigravity should later formalize a machine-readable compatibility matrix, for example:

```yaml
profiles:
  single_gpu_24gb:
    supported_os:
      - windows_wsl2
      - linux
    gpu:
      min_vram_gb: 20
    packages:
      torch: "validated-version"
      cuda: "validated-version"
      livetalking: "pinned-revision"
      qwen3_tts: "pinned-revision"
      faster_whisper: "pinned-version"
    models:
      llm: balanced
      stt: adaptive
      lipsync: wav2lip
```

The compatibility matrix is planning intent; exact schemas/paths belong to Antigravity/OpenSpec resolution.

## Preflight Result Classes

Every check should return one of:

```text
PASS
WARN
AUTO_FIX
FALLBACK_REQUIRED
BLOCKED
```

Examples:

```text
CUDA compatible                         PASS
ffmpeg missing                          AUTO_FIX
24 GB VRAM / MuseTalk + local 14B       FALLBACK_REQUIRED
port 8080 excluded by Hyper-V           AUTO_FIX → choose 8090
microphone over insecure LAN HTTP       BLOCKED → localhost/HTTPS required
private voice reference missing         WARN → default voice profile
avatar worker unavailable               FALLBACK_REQUIRED → voice/text mode
```

## Install Plan Must Be Visible Before Destructive Changes

The resolver should emit a human-readable plan such as:

```text
Detected: RTX 4090 24GB / RAM 64GB / WSL2 Ubuntu 24.04

Recommended profile: BALANCED-24G

LLM       Qwen quantized local      OK
STT       faster-whisper medium     selected for VRAM headroom
TTS       Qwen3-TTS                 OK
Avatar    LiveTalking               OK
Lip Sync  Wav2Lip                   selected instead of MuseTalk
WebRTC    port 8010 available       OK

Estimated GPU budget: within validated target
Fallback mode: text + voice remains available
```

The system may auto-apply safe fixes, but should record what was changed.

## Two-Stage Doctor

Recommended operator model:

```text
awin doctor --preflight
    ↓
capability report + recommended profile

awin bootstrap --profile auto
    ↓
install selected stack

awin doctor --runtime
    ↓
model load + service + WebRTC + microphone + smoke tests
```

Names are illustrative; Antigravity may adapt CLI conventions to the repository.

## Runtime Re-evaluation

Preflight is not one-time only. Re-run profile resolution when:

- GPU changes;
- CUDA/driver changes;
- dependency upgrades occur;
- model profile changes;
- avatar engine changes;
- a previous service repeatedly OOMs;
- performance falls below defined realtime thresholds.

Runtime may suggest a safer profile, but must not silently change important providers/models without recording the transition.

## Failure / OOM Recovery

If runtime measurements contradict preflight estimates:

```text
OOM / startup failure / FPS below threshold
        ↓
collect evidence
        ↓
mark current profile unhealthy
        ↓
select next validated fallback
        ↓
restart affected services
        ↓
health test
        ↓
record fallback event
```

Do not loop indefinitely. A bounded fallback chain must eventually reach voice-only/text-only degraded mode or BLOCKED status.

## AI Company Governance

This capability follows the bidirectional planning model:

```text
Chairman + ChatGPT planning
        ↓
_myplan_ decision/discussion
        ↓
Antigravity repository inspection
        ↓
OpenSpec ownership resolution
        ↓
implementation + measurements
        ↓
_myplan_/feedback if assumptions were wrong
```

ChatGPT does not assign an OpenSpec Change ID for this capability.

## Acceptance Intent

Future formal implementation should demonstrate that:

1. installation starts with capability detection;
2. incompatible/high-resource profiles are rejected before model installation where possible;
3. compatible lower-resource providers/models are selected automatically;
4. safe missing dependencies can be automatically installed/fixed;
5. package/model versions come from a validated compatibility matrix;
6. the chosen profile and reasons are visible to the Chairman/operator;
7. runtime health is tested after installation;
8. OOM/performance failure can trigger a bounded fallback path;
9. voice-only and text-only modes remain valid terminal fallbacks;
10. all automatic substitutions and fixes are recorded for audit/reproducibility.
