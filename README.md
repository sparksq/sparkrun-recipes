# ColdSnap Sparkrun recipes

This repository holds qualified ColdSnap recipes and their equivalent vanilla
controls. ColdSnap artifacts, model weights, and OCI capsules live outside the
repository.

## Contents

| Model and engine | ColdSnap recipe | Matched vanilla recipe | Qualified driver |
| --- | --- | --- | --- |
| Qwen3.8-27B-FP8 TP2 / vLLM | `coldsnap-recipes/qwen3.8-27b-fp8-coldsnap-tp2-vllm.yaml` | `vanilla-recipes/qwen3.8-27b-fp8-vanilla-tp2-vllm.yaml` | n580, n610 |
| Qwen3.8-27B-FP8 TP2 / SGLang | `coldsnap-recipes/qwen3.8-27b-fp8-coldsnap-tp2-sglang.yaml` | `vanilla-recipes/qwen3.8-27b-fp8-vanilla-tp2-sglang.yaml` | n580, n610 |
| Qwen3.8-27B-NVFP4 DSpark TP2 / SGLang | `coldsnap-recipes/qwen3.8-27b-nvfp4-dspark-coldsnap-tp2-sglang.yaml` | `vanilla-recipes/qwen3.8-27b-nvfp4-dspark-vanilla-tp2-sglang.yaml` | n580, n610 |
| DeepSeek-V4-Flash-0731 TP2 / vLLM | `coldsnap-recipes/deepseek-v4-flash-0731-coldsnap-tp2-vllm.yaml` | `vanilla-recipes/deepseek-v4-flash-0731-vanilla-tp2-vllm.yaml` | n580, n610 |

Each vanilla control intentionally matches its ColdSnap counterpart's model,
revision, image, topology, environment, and engine launch arguments. The
vanilla file differs only by omitting the ColdSnap builder and configuration
and by describing itself as a control. Preserve that pairing when changing a
recipe.

- `coldsnap-recipes/` contains capture and restore recipes.
- `vanilla-recipes/` contains matched InstantTensor controls.
- `.sparkrun/registry.yaml` exposes both directories as hidden registries.
