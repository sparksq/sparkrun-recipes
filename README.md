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
- `evidence/` is reserved for ignored local qualification output.

## Use

The current recipes require exactly two NVIDIA/CUDA nodes and a current
Sparkrun installation with the first-party ColdSnap plugin. Sparkrun selects
the compatible n580 or n610 ColdSnap implementation from the detected driver.

```bash
coldsnap_recipe=coldsnap-recipes/qwen3.8-27b-fp8-coldsnap-tp2-vllm.yaml
vanilla_recipe=vanilla-recipes/qwen3.8-27b-fp8-vanilla-tp2-vllm.yaml
cluster=g610

sparkrun recipe validate "$coldsnap_recipe"
sparkrun recipe validate "$vanilla_recipe"
sparkrun run "$vanilla_recipe" --cluster "$cluster"
sparkrun coldsnap capture "$coldsnap_recipe" --cluster "$cluster" --render-only
sparkrun coldsnap capture "$coldsnap_recipe" --cluster "$cluster"
sparkrun coldsnap publish "$coldsnap_recipe" --cluster "$cluster"
sparkrun run "$coldsnap_recipe" --cluster "$cluster"
```

Capture creates and activates local capsules first. After validation,
`publish` pushes capsules to their configured `docker.io/scitrera/...`
repositories, publishes the descriptor in the same OCI repository, and
activates the digest-pinned portable descriptor. `sparkrun run` automatically
uses ColdSnap restore for a ColdSnap recipe; `sparkrun coldsnap restore` is
available for explicit weight-mode overrides.
