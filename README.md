# Krea2 BBOX Prompter Suite

<img width="1722" height="868" alt="Krea2 BBOX Prompter Suite" src="https://github.com/user-attachments/assets/4da38a62-cc7d-46b9-8b78-cdad7053eac3" />

Krea2 BBOX Prompter Suite is a ComfyUI custom-node suite for manually placing BBOX regions and converting them into a Krea2-oriented JSON prompt. Heavily leaning on the implementation by https://github.com/ukr8b3g-cmyk

Improvements include
- full natural language option without any JSON making it way into the prompt/image generation
- better auto-descriptions based on the spatial awareness (e.g. smaller boxes near the top are further set back unless prompted otherwise)
- Resizing BBOX rectangles much easier now

Draw the layout, write a prompt for each color slot, export the result, and optionally append a style effect or one of the 592 local background presets.

> BBOX regions are layout guidance, not strict masks. Final placement still depends on the model, prompt wording, and scene context.

## At a glance

| Node | Purpose | Main output |
| --- | --- | --- |
| Krea2 BBOX Canvas | Set the image size and draw colored BBOX regions | framing_data |
| Krea2 BBOX Prompter | Write Scene, Background, Object, and Text prompts | prompt_ui_data |
| Krea2 BBOX Export | Combine layout and prompts into Krea2 JSON | prompt_text, width, height |
| Krea2 BBOX Prompt Effect | Append photo, art, lighting, weather, color, finish, or mood text | prompt_out, effect_text |
| Krea2 Background Effect | Append a selected background preset or custom background text | prompt_out, background_text |

## Highlights

- Five color slots: RED, BLUE, YELLOW, GREEN, and MAGENTA.
- Object and Text slot types with framing and camera-angle hints.
- Optional automatic position hints derived from each BBOX.
- 592 Background Effect presets with local WebP thumbnails.
- Optional Photo or Anime Style Boost on the Prompt Effect node.
- Custom Prompt Effect presets saved locally.
- No LLM, OCR, image analysis, or automatic BBOX detection.
- No model download is performed by this extension.

## Quick start

### Install from GitHub

    cd ComfyUI/custom_nodes
    git clone https://github.com/CaptainGrock/Krea2bbox.git

Restart ComfyUI. If the old interface remains, hard-refresh the browser with Ctrl + F5.

### Five-minute workflow

1. Add Krea2 BBOX Canvas and choose the generation size.
2. Draw one or more BBOX regions.
3. Add Krea2 BBOX Prompter and write Scene, Background, and slot prompts.
4. Connect Canvas.framing_data and Prompter.prompt_ui_data to Krea2 BBOX Export.
5. Send Export.prompt_text to Krea2 BBOX Prompt Effect or CLIP Text Encode.
6. Connect Export.width and Export.height to EmptyLatentImage.
7. Optionally use Krea2 Background Effect when a preset background should be appended.


### Text slots

Use Text only when the generated image should contain visible writing. The preferred format is:

    visible text | appearance description

Example:

    SALE | large red text with a white outline

The part before the separator becomes the text field. The part after it becomes the visual description.

### BBOX guidance

Start with one or two boxes. Avoid strong overlap unless the overlap is intentional. The BBOX order is [x1, y1, x2, y2].

BBOX placement is guidance for Krea2, not a pixel-accurate mask. If the result is weak, adjust the box, the prompt wording, and the amount of scene context together.

### Krea2 Background Effect

Background Effect is a separate, background-focused node. It loads 592 presets from the bundled local data and appends the selected text to prompt_in.

Use it when you want a ready-made environment description without changing the BBOX JSON. The node has no model-specific dependency and does not require a separate download.

### Custom effects

Select Custom, edit the effect text, and save it as a local preset. Built-in presets are read-only; use Copy to Custom before changing one.

Custom presets are stored at:

    user_presets/prompt_effect_custom_presets.json

The file is created when the first custom effect is saved.

## Output connections

    Canvas.framing_data
              |
    Prompter.prompt_ui_data
              v
    Krea2 BBOX Export.prompt_text
              |
              v
    Krea2 BBOX Prompt Effect.prompt_in
              |
              v
    CLIP Text Encode.text

Export.width and Export.height should be connected to the corresponding EmptyLatentImage inputs.

Background Effect can receive a prompt string through prompt_in and returns prompt_out plus background_text.

## License

See the repository license and distribution terms.

## Original Author

https://github.com/ukr8b3g-cmyk/Krea2-BBOX-Prompter
