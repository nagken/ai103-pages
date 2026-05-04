# Domain 3 — Implement Computer Vision Solutions (10–15%)

> Computer vision on AI-103 is **not just analysis**. It now covers **image generation, video generation, multimodal understanding, Content Understanding for visual data, accessibility (alt-text), and visual responsible AI** (indirect prompt injection embedded in images, watermarking, brand policy).

## Mind map

```mermaid
mindmap
  root((Computer Vision))
    Image generation
      Text to image
      Reference media
      Inpainting
      Mask edits
      Prompt-driven edits
    Video generation
      Text to video
      Reference clips
      Storyboards
      Edit workflows
    Multimodal understanding
      Vision-LLM Q&A
      Captions short + detailed
      Visual grounding
      Video segments
    Content Understanding visual
      Single-task
      Pro-mode
      Object + region extraction
      Visual characteristics
    Accessibility
      Alt text
      Extended descriptions
    Responsible AI
      Unsafe content filters
      Indirect prompt injection in images
      Watermark + brand rules
```

## Image and video generation

```mermaid
flowchart LR
    PROMPT[Text prompt] --> IMG[Image generation model]
    REF[Reference image] --> IMG
    IMG --> OUT[Generated image]
    OUT --> EDIT{Need edits?}
    EDIT -- Inpainting --> MASK[Mask region + new prompt]
    MASK --> IMG2[Regenerate that region]
    EDIT -- Prompt edit --> IMG3[Re-prompt with style/content change]
    PROMPT2[Text + storyboard] --> VID[Video generation model]
    REF2[Reference clip] --> VID
    VID --> CUT[Edit / trim / restyle workflow]
```

| Task | Pick |
| --- | --- |
| Generate marketing image from prompt | **Image generation model** in Foundry |
| Edit only the sky in an existing image | **Inpainting with mask** |
| Replace product color while keeping pose | **Mask-based prompt edit** |
| Generate short clip from script | **Video generation model** |
| Edit a generated clip (trim, restyle, swap subject) | **Video edit workflow on the generated asset** |

> Trap: do not pick "Custom Vision training" for generation. Custom Vision = **classify or detect** in **existing** images. Image **generation** is a separate model class in Foundry.

## Multimodal understanding workflows

```mermaid
flowchart TD
    A[Image or video input] --> M[Multimodal model in Foundry]
    M --> B{Goal}
    B -- Caption --> C[Concise OR detailed caption]
    B -- Visual Q+A --> D[Answer grounded in image evidence]
    B -- Alt text / accessibility --> E[Alt text + extended description]
    B -- Object / region --> F[Identify objects, components, regions]
    A --> CU[Content Understanding visual analyzer]
    CU --> SINGLE[Single-task: tags, objects, captions]
    CU --> PRO[Pro-mode: structured fields, multi-step reasoning]
```

| Need | Service | Notes |
| --- | --- | --- |
| Free-form caption / Q&A | **Multimodal LLM** | Best for natural language |
| Structured visual extraction (fields, tags, scores) | **Content Understanding** analyzer | Single-task or pro-mode |
| Long video — find moments, segments, scenes | **Video analysis pipeline** with Content Understanding | Per-segment outputs |
| Accessible alt-text + extended description | **Multimodal LLM with accessibility prompt** | Aligned to WCAG-style guidance |
| Detect specific products / parts in images | **Object / region extraction** via Content Understanding or vision-LLM grounding |

## Content Understanding — single-task vs pro-mode

| Mode | Use it when |
| --- | --- |
| **Single-task** | One narrow output (e.g., "extract make/model"); fast, cheap, low complexity |
| **Pro-mode** | Multi-step reasoning, multi-field extraction, complex layouts, mixed modalities; richer structured output |

> Trap: "Extract a single tag from each image at high volume" → **single-task**. "Extract structured JSON with 12 fields and visual reasoning" → **pro-mode**.

## Visual responsible AI

```mermaid
flowchart LR
    IN[Image input] --> SAFE[Visual content filter - hate, violence, sexual, self-harm]
    SAFE --> INJ[Detect indirect prompt injection - text embedded in image]
    INJ --> POL[Brand / watermark / symbol policy]
    POL --> M[Multimodal model]
    M --> OUT[Response or refusal]
    GEN[Generated image] --> WM[Apply watermark + provenance]
```

Required controls on AI-103:

- **Visual content filters** classify and block unsafe imagery.
- **Indirect prompt injection** — text inside an image that tries to override the system prompt is detected and stripped.
- **Visual policy enforcement** — watermarking, brand-asset rules, prohibited symbols, "potentially inappropriate" flagging.
- **Provenance metadata** on generated assets (who / what model / when).

## Service decision flow

```mermaid
flowchart TD
    Q[Visual scenario] --> G{Generate or understand?}
    G -- Generate image --> IG[Image generation model]
    G -- Generate video --> VG[Video generation model]
    G -- Understand --> U{Structured or freeform output?}
    U -- Freeform answer / caption / alt text --> ML[Multimodal LLM]
    U -- Structured fields / tags --> CU{Single field or many?}
    CU -- One narrow output --> ST[Content Understanding single-task]
    CU -- Many fields, complex --> PRO[Content Understanding pro-mode]
    U -- Long video, find segments --> VID[Video analysis with Content Understanding]
```

## Domain summary

- **Generation** (image/video) and **understanding** (multimodal) are separate model classes.
- **Inpainting + mask edits** are the canonical answer for "change only this region".
- **Content Understanding** owns structured visual extraction; pick **single-task** vs **pro-mode** by complexity.
- **Multimodal LLMs** own free-form captions, Q&A, alt-text, and visual grounding.
- **Visual safety** = unsafe-content filters **plus** indirect-prompt-injection detection **plus** brand / watermark policy.
