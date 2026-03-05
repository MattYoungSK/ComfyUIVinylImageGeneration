# 🎵 Vinyl Record Product Photography — ComfyUI Workflow

A ComfyUI workflow that transforms any album cover image into a professional, studio-quality product photograph using **FLUX.2 Klein** with image-to-image reference generation.

---

## 🖼️ What It Does

Takes an uploaded album cover photo and regenerates it as a **minimalist, high-end product shot** — clean white background, soft studio lighting, glossy surface reflection — in a Scandinavian product photography style. All artwork, text and graphics from the original cover are preserved faithfully.

---

## 🧠 Model Stack

| Component | Model |
|---|---|
| Diffusion Model | `flux-2-klein-base-9b-fp8.safetensors` |
| Text Encoder (CLIP) | `qwen_3_8b_fp8mixed.safetensors` |
| VAE | `flux2-vae.safetensors` |
| Upscaler | `realesrgan_x2plus.pth` |

---

## ⚙️ Workflow Architecture
```
[LoadImage] → [ImageScaleToTotalPixels (1MP)]
                        ↓
                  [VAEEncode]
                  ↙         ↘
     [ReferenceLatent]   [ReferenceLatent]
      (positive cond.)    (negative cond.)
                  ↘         ↙
              [KSamplerAdvanced]
                      ↓
                 [VAEDecode]
                      ↓
         [ImageUpscaleWithModel (RealESRGAN x2)]
                      ↓
              [SaveImageExtended]
```

The workflow uses **ReferenceLatent** conditioning — the input image is encoded into latent space and used to anchor both positive and negative conditioning paths, giving the model a strong visual reference while the prompt reshapes the scene.

---

## 🔧 Key Parameters

| Parameter | Value |
|---|---|
| Sampler | Euler |
| Scheduler | Simple |
| Steps | 8 |
| CFG | 1.0 |
| Guidance (FluxGuidance) | 3.5 |
| Input resolution | ~1 MP (normalized) |
| Output | 2× upscaled `.jpg` (quality 100) |

---

## 📦 Requirements

- [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
- Custom nodes:
  - `cg-use-everywhere` (Anything Everywhere)
  - `save-image-extended-comfyui`
- Models downloaded to their respective ComfyUI directories (links embedded in node metadata inside the `.json`)

---

## 🚀 Usage

1. Clone this repo
2. Drag the `.json` workflow file into the ComfyUI canvas
3. Download the required models (links are in the node metadata)
4. Upload your album cover image to the **LoadImage** node
5. Queue the prompt — output saves to `ComfyUI/output/PRODUCT_IMGS/`

---

## 💡 Prompt

The positive prompt uses the `[IMG-1]` reference token to link the input image into text conditioning. Default prompt instructs the model to produce a pure white seamless studio backdrop, glossy reflective surface, soft diffused lighting, with all original album artwork preserved exactly.

You can swap the prompt to reuse this workflow for other product types (books, bottles, gadgets, etc.).

---

*Built as part of a creative/technical portfolio demonstrating AI-assisted product photography pipelines.*
