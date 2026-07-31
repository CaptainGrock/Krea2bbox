# Krea2 BBOX Prompter Suite

<img width="1722" height="868" alt="Krea2 BBOX Prompter Suite" src="https://github.com/user-attachments/assets/4da38a62-cc7d-46b9-8b78-cdad7053eac3" />

Krea2 BBOX Prompter Suite is a ComfyUI custom-node suite for manually placing BBOX regions and converting them into a Krea2-oriented JSON prompt. This tweaked version heavily relies on original coding here (want to make sure I give credit where credit is due!): https://github.com/ukr8b3g-cmyk/Krea2-BBOX-Prompter

Improvements/changes I've made to this version:
- Heavily updated the end prompt to remove any JSON or non-existent prompt language so it didn't end up in the image output
- Updated the spatial awareness logic so it better outputs where composition is and also boxes are next to / around each other
- Updated the box drawing to be easier to resize (click on one and handlers appear to resize easily)
- Added optional override inputs to the definitions area so you can just pipe in the regions text (e.g. from wildcards, etc.) but still allow the bboxes to control the composition

## General Instructions
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

The Suite and Pose Gesture V1 are intentionally installed as two independent custom-node folders. For a new installation:

    cd ComfyUI/custom_nodes
    git clone https://github.com/CaptainGrock/Krea2bbox
    

### Five-minute workflow

1. Add Krea2 BBOX Canvas and choose the generation size.
2. Draw one or more BBOX regions.
3. Add Krea2 BBOX Prompter and write Scene, Background, and slot prompts.
4. Connect Canvas.framing_data and Prompter.prompt_ui_data to Krea2 BBOX Export.
5. Send Export.prompt_text to Krea2 BBOX Prompt Effect or CLIP Text Encode.
6. Connect Export.width and Export.height to EmptyLatentImage.
7. Optionally use Krea2 Background Effect when a preset background should be appended.


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

## Limitations and practical notes

- Krea2 may not follow every BBOX exactly.
- Conflicting framing or angle hints can weaken the result.
- Text rendering depends on the model and prompt context.
- This suite does not translate Japanese, expand short prompts with an LLM, read images, run OCR, or detect BBOX regions automatically.
- After updating the front-end JavaScript, restart ComfyUI and press Ctrl + F5.
- Optional autocomplete is used only when ComfyUI-Custom-Scripts is already installed; it is not required.

## Technical reference

### Node IDs

    Krea2ElementFramingV1Canvas
    Krea2ElementFramingV1Prompt
    Krea2ElementJSONExportV1
    Krea2BBOXPromptEffect
    Krea2BackgroundEffect

## License

See the repository license and distribution terms.

## Original Author

https://github.com/ukr8b3g-cmyk/Krea2-BBOX-Prompter
