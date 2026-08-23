# ColdSnap Sparkrun recipes

This repository is a Sparkrun registry of qualified ColdSnap capture and
restore recipes. ColdSnap artifacts, model weights, and OCI capsules are stored
outside the repository.

## Contents

| Recipe | Model | Shape | Weight policy |
| --- | --- | --- | --- |
| `qwen3.8-27b-fp8-coldsnap-tp2.yaml` | Qwen3.8-27B-FP8 | TP2 | Recovery safetensors |
| `deepseek-v4-flash-0731-coldsnap-tp2.yaml` | DeepSeek-V4-Flash-0731 | TP2 | Native preferred, recovery fallback |

- `coldsnap-recipes/` contains the recipe YAML files.
- `.sparkrun/registry.yaml` exposes them as the `coldsnap` registry.
- `evidence/` is reserved for ignored local qualification output.

## Use

The current recipes require two NVIDIA/CUDA nodes with NVIDIA driver 610 or
newer and a current Sparkrun installation with the first-party ColdSnap plugin.

```bash
recipe=coldsnap-recipes/qwen3.8-27b-fp8-coldsnap-tp2.yaml
cluster=g610

sparkrun recipe validate "$recipe"
sparkrun coldsnap capture "$recipe" --cluster "$cluster" --render-only
sparkrun coldsnap capture "$recipe" --cluster "$cluster"
sparkrun run "$recipe" --cluster "$cluster"
```

After capture, `sparkrun run` selects ColdSnap restore automatically. Use
`sparkrun coldsnap restore` when an explicit restore command or override is
needed.

## Publishing

The checked-in OCI references target the `localhost:5500` g610 qualification
registry, so the catalog remains hidden from default registry listings. Before
publishing it, mirror those images and capsule repositories to an accessible
namespace, preserve immutable image digests, and remove `visible: false` from
the registry manifest.
