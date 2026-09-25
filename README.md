# GLSL

A collection of raw WebGL / GLSL shader experiments — each one a self-contained HTML file you can open directly in a browser.

**Repository:** [razodin137/GLSL](https://github.com/razodin137/GLSL)

## Shaders

Live in the [`shaders/`](./shaders) directory:

- [rainbowshader.html](./shaders/rainbowshader.html) — "Neon Goo Landing": a rainbow animated goo effect.
- [liquidrainbow-text-shader.html](./shaders/liquidrainbow-text-shader.html) — liquid rainbow text shader.
- [nebula-heatmap.html](./shaders/nebula-heatmap.html) — a nebula heatmap with noise functions, reacting to resolution, mouse, and time.

Each file contains its own vertex and fragment shaders plus the JavaScript to run them — no build step, no dependencies.

## System prompt

- [system-prompts/GLSL-creator-system-prompt.md](./system-prompts/GLSL-creator-system-prompt.md) — a system prompt for an expert creative coder / graphics engineer persona that outputs single self-contained HTML files with CSS, JavaScript, and GLSL. This is the prompt that generated the shaders in this repo.