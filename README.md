#  Text-to-Image Generator with Stable Diffusion

A lightweight, theme-based text-to-image generation pipeline built with **Stable Diffusion v1.5** and Hugging Face `diffusers`. Pick a theme, generate a batch of curated-prompt images, and save the results locally — all from a single notebook.

---

##  Features

- **Interactive theme picker** — dropdown widget to choose between 5 curated themes: `animal`, `food`, `science`, `landscape`, and `wildcard`
- **Hand-crafted prompt bank** — each theme ships with multiple pre-written, photography/art-style prompts tuned for high-quality Stable Diffusion output
- **Batch generation** — generates multiple images per run with randomized prompt selection and random seeds for variety
- **Reproducibility** — every generated image is saved with its prompt and seed baked into the filename, so any result can be reproduced exactly
- **GPU/CPU auto-detection** — automatically uses CUDA if available, falls back to CPU otherwise
- **Organized output** — images are saved into theme-named subfolders under `outputs/`

---

##  How It Works

1. **Model**: [`runwayml/stable-diffusion-v1-5`](https://huggingface.co/runwayml/stable-diffusion-v1-5) loaded via `diffusers.StableDiffusionPipeline`
2. **Theme selection**: An `ipywidgets` dropdown sets a global `theme` variable
3. **Prompt sampling**: For each image, a prompt is randomly drawn from that theme's prompt list, and a random seed is generated
4. **Inference**: Each image is generated with:
   - `guidance_scale = 7.5`
   - `num_inference_steps = 30`
5. **Output**: Images are saved as PNGs to `outputs/<theme>/<theme>_<index>_seed<seed>.png` and displayed inline with their prompt/seed metadata

---

##  Project Structure

```
text_to_image_animal.ipynb   # Main notebook
outputs/
  ├── animal/
  ├── food/
  ├── science/
  ├── landscape/
  └── wildcard/
```

---

##  Setup

This notebook is built for **Google Colab** with a GPU runtime (T4 or better recommended).

1. Open the notebook in Colab and set the runtime to **GPU** (`Runtime > Change runtime type > T4 GPU`)
2. Run the dependency install cell:
   ```bash
   pip install -qqU diffusers transformers bitsandbytes accelerate ftfy datasets
   ```
3. If `stable-diffusion-v1-5` requires authentication in your environment, log into Hugging Face first:
   ```python
   from huggingface_hub import login
   login()  # or set the HUGGINGFACE_HUB_TOKEN env variable
   ```

---

##  Usage

1. **Run all cells top to bottom.**
2. Use the dropdown widget to pick a theme (defaults to `food` if untouched).
3. The generation cell will:
   - Pick `num_images` (default: 4) random prompts from the selected theme
   - Generate one image per prompt with a random seed
   - Save each to `outputs/<theme>/`
   - Display each image inline along with its prompt, seed, and file path

**To tweak the output:**
| Parameter | Location | Effect |
|---|---|---|
| `theme` | dropdown / can be set directly | Which prompt bank to sample from |
| `num_images` | generation cell | How many images to generate per run |
| `guidance_scale` | generation cell | Higher = more prompt-adherent, less creative (default `7.5`) |
| `num_inference_steps` | generation cell | More steps = higher quality, slower (default `30`) |

---

##  Sample Results

> Run the notebook and drop your generated images here — e.g.:
>
> | Prompt | Seed | Image |
> |---|---|---|
> | *portrait of a red fox in a forest, shallow depth of field, 85mm lens, bokeh* | `1234` | `outputs/animal/animal_1_seed1234.png` |
> | *majestic lion at golden hour on the savannah, dramatic lighting, ultra realistic* | `5678` | `outputs/animal/animal_2_seed5678.png` |

*(Placeholder — populate this section with your actual generated images once you've run the notebook.)*

---

##  Possible Extensions

- Add a prompt input box for fully custom prompts alongside the theme presets
- Support additional/newer models (e.g. SDXL, SD 2.1) via a model-picker dropdown
- Add negative prompts to steer away from common artifacts
- Log all generations (prompt, seed, params) to a CSV/JSON manifest for easier dataset curation
- Build a small Gradio/Streamlit UI on top of the same pipeline for non-notebook use

---

##  Requirements

- Python 3.x
- `torch`, `diffusers`, `transformers`, `accelerate`, `bitsandbytes`, `ftfy`, `datasets`
- GPU strongly recommended (CPU inference is very slow for Stable Diffusion)
