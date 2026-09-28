# Anthropic Prompt Template

The **Summarize with Claude** node sends one request per paper to the Anthropic Messages API. Below is the full prompt structure.

## System message

```
You are a research digest assistant. Return only valid JSON, no markdown fences.
```

## User message

```
Research profile: Deep learning for medical imaging, code generation LLMs, and multi-agent systems.

Summarize the following arXiv paper in 2-3 plain-language sentences
and rate its relevance to the research profile on a 1-10 scale.

Return ONLY valid JSON (no markdown fences, no preamble):
{
  "summary": "<2-3 sentence summary>",
  "relevance_score": <integer 1-10>,
  "tags": ["<tag1>", "<tag2>", "<tag3>"]
}

Title: {title}
Authors: {authors}
Abstract: {abstract}
Topic bucket: {topicLabel}
```

## Expected response

```json
{
  "summary": "This paper introduces a transformer-based architecture for segmenting cardiac MRI scans, achieving state-of-the-art Dice scores on the ACDC benchmark. The method uses a hybrid CNN-transformer encoder with a lightweight decoder that runs in real time on consumer GPUs.",
  "relevance_score": 9,
  "tags": ["cardiac-MRI", "segmentation", "transformer", "real-time-inference"]
}
```

## Model parameters

| Parameter | Value | Rationale |
|---|---|---|
| `model` | `claude-sonnet-4-6` | Good balance of quality and cost for structured extraction. |
| `max_tokens` | 400 | Enough for the JSON payload; keeps cost down. |
| `temperature` | 0.2 | Low temperature for consistent, deterministic JSON output. |

## Scoring guidelines

The prompt does not prescribe a rubric — Claude infers relevance from the research profile string. In practice the scores cluster as follows:

| Range | Meaning |
|---|---|
| 9–10 | Directly advances one of the three profile topics. |
| 7–8 | Closely related method or application. |
| 5–6 | Tangentially related (shared technique, different domain). |
| 1–4 | Minimal overlap with the research profile. |

## Customization

To change the research profile, edit the `RESEARCH_PROFILE` constant in the **Build LLM Prompt** node. The prompt template itself lives in the same node as a JavaScript template literal.
