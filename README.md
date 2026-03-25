<p align="center">
  <img src="quizforge.png" width="120" alt="QuizForge icon"/>
</p>

<h1 align="center">QuizForge</h1>

<p align="center">
  <strong>Turn any PDF into an interactive quiz — powered by local AI.</strong><br/>
  No cloud, no API keys, no data leaves your machine.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey" alt="Platform"/>
  <img src="https://img.shields.io/badge/AI-100%25%20local-00e5c8" alt="Local AI"/>
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License"/>
</p>

---

## What is QuizForge?

QuizForge reads any PDF document, extracts the text, and uses artificial intelligence running **entirely on your computer** to generate multiple-choice quiz questions. You answer them in a clean web interface, get scored at the end, and receive AI-generated explanations for every mistake — all without sending a single byte to the internet.

---

## How to use it

### 1. Install Ollama (one time only)

QuizForge needs **Ollama** to run AI models locally on your machine. Ollama is a free, open-source tool that downloads and runs language models on your hardware.

👉 **Download Ollama**: [https://ollama.com/download](https://ollama.com/download)

Install it, then you can forget about it — QuizForge starts and stops it automatically.

### 2. Launch QuizForge

Double-click `QuizForge-IT.exe` (Italian) or `QuizForge-EN.exe` (English). That's it.

On the **first launch**, you'll see a splash screen that:
- Checks if Ollama is installed and starts it
- Downloads the AI model (~4.7 GB, one time only)
- Shows a progress bar during the download

Once ready, your browser opens automatically with the quiz interface.

### 3. When you're done

Just **close the browser tab**. QuizForge detects the tab is closed and shuts down automatically, including Ollama. No background processes left running.

---

## What you'll see in the interface

### Sidebar (left panel)

This is where you configure and launch the quiz generation.

| Element | What it does |
|---|---|
| **Upload PDF** | Drag a PDF here or click to browse. This is the document the AI will read and create questions from. Works with any PDF that contains selectable text (textbooks, slides, reports, papers…). |
| **Questions** | How many quiz questions to generate (5–100). More questions = longer generation time. 20 is a good default. |
| **Difficulty** | **Easy**: basic concepts, obvious wrong answers. **Medium**: important concepts, plausible wrong answers. **Hard**: in-depth questions, tricky distractors that require careful reading. |
| **Model** | Which AI model to use. **Auto** (recommended): balanced speed and quality. **Fast**: smaller model, quicker but slightly less accurate. **Accurate**: larger model, slower but higher quality questions. |
| **Chunk** | The AI reads the PDF in pieces called "chunks". This number (default: 3500) is how many characters per piece. **Lower** = more pieces, more questions spread across the document, slower. **Higher** = fewer pieces, faster, but might miss details in long documents. Leave at 3500 unless you have a reason to change it. |
| **OCR for scanned PDFs** | Toggle this ON only if your PDF is a scanned image (photos of pages). Requires Tesseract + Poppler installed on your system. For normal PDFs (selectable text), leave it OFF — it's not needed. |
| **Generate Quiz** | Starts the process. Questions appear in real time as the AI generates them — you can start answering before it finishes. |
| **Check Ollama** | Manually checks if Ollama is running and which model is loaded. The status chips below update automatically. |

### Status chips (bottom of sidebar)

| Chip | Meaning |
|---|---|
| **Ollama** | ✓ Active = Ollama is running. ✗ Not active = something is wrong. |
| **Model** | Which AI model is selected. |
| **Deps** | ✓ OK = everything fine. ⚠ tesseract = OCR dependencies missing (only matters if you need OCR). |

### Question map (top bar)

When a quiz is active, a bar appears at the top with:
- **Numbered dots** — one per question. Click any dot to jump to that question.
- **Colors**: blue = answered, current = teal outline, after finishing: green = correct, red = wrong, yellow = skipped.
- **Pulsing dot** = the AI is generating an explanation for that question in the background.
- **Timer** — tracks how long you've been on the quiz.

### Quiz area (main panel)

- **Question text** — the question generated from your PDF content.
- **Options 1–4** — click to select, or press `1` `2` `3` `4` on your keyboard.
- **Navigation** — "Back" / "Next" buttons, or use `←` `→` arrow keys.
- **"More questions loading…"** — appears if you reach the last generated question while the AI is still working. Wait a moment and more will appear.
- **"Finish quiz"** — only appears on the last question when generation is complete.

### Results screen

After finishing:
- **Score ring** — animated percentage with color: green (≥70%), yellow (40–69%), red (<40%).
- **Stats** — correct, wrong, and skipped counts.
- **AI Explanation** — the AI explains why each wrong answer was wrong, with the correct answer and reasoning.
- **Review answers** — go back through every question. Wrong answers are highlighted in red, correct ones in green, with individual AI explanations shown below each question.
- **Export JSON** — downloads a file with all questions, your answers, and correctness. Useful for studying or sharing.
- **New quiz** — reset everything and start fresh.

### Keyboard shortcuts

| Key | Action |
|---|---|
| `←` or `A` | Previous question |
| `→` or `D` | Next question |
| `1` `2` `3` `4` | Select answer |
| `Enter` | Finish quiz (on last question) |
| `Esc` | Back to results (in review mode) |

---

## The AI behind QuizForge

### What is Ollama?

[Ollama](https://ollama.com) is a tool that lets you run large language models (LLMs) locally on your computer. Think of it as ChatGPT, but running entirely on your machine — no internet required, no data shared, completely private.

QuizForge starts and stops Ollama automatically. You just need to install it once.

### What model does it use?

QuizForge uses **Qwen 2.5** by default, an open-source language model developed by Alibaba Cloud. It's chosen because:
- **Fast** — no "thinking chain" overhead, generates structured output quickly
- **Reliable JSON** — produces clean, parseable quiz data with minimal errors
- **Good multilingual support** — works well in both English and Italian
- **Reasonable size** — the 7B (7 billion parameter) version is ~4.7 GB

| Mode in the UI | Model | Download size | Best for |
|---|---|---|---|
| **Auto** | qwen2.5:7b | ~4.7 GB | General use, good balance |
| **Fast** | qwen2.5:3b | ~2.0 GB | Quick quizzes, less powerful hardware |
| **Accurate** | qwen2.5:14b | ~9.0 GB | Best quality, needs more RAM/VRAM |

### How the quiz generation works

1. **Text extraction** — `pdfplumber` reads the text from your PDF. No AI involved here, just parsing.
2. **Chunking** — the text is split into pieces (default ~3500 characters each) so the AI can process them.
3. **Question generation** — each chunk is sent to the AI with a prompt asking for multiple-choice questions. The AI is forced to respond in a strict JSON format, so the output is always structured and parseable.
4. **Filtering** — questions about page numbers, chapter titles, or document structure are automatically removed.
5. **Streaming** — questions are sent to your browser one by one as they're generated. You can start answering immediately.
6. **Explanations** — when you answer wrong, the question is queued in the background. The AI generates a personalized explanation while you continue the quiz. By the time you finish, most explanations are already ready.

### Hardware requirements

Ollama runs on CPU by default, but uses GPU if available for much faster generation.

| Model | Minimum RAM | Recommended |
|---|---|---|
| qwen2.5:3b | 4 GB | 8 GB |
| qwen2.5:7b | 8 GB | 16 GB |
| qwen2.5:14b | 16 GB | 32 GB or GPU with 10+ GB VRAM |

---

## OCR — when do you need it?

**Most PDFs don't need OCR.** If you can select and copy text from your PDF, OCR is not needed.

OCR is only for PDFs that are **scanned images** — where each page is a photo of paper. These PDFs contain no actual text data, just images.

If you need OCR, install:
- **[Tesseract](https://github.com/UB-Mannheim/tesseract/wiki)** — the OCR engine
- **[Poppler](https://github.com/oschwartz10612/poppler-windows/releases)** — converts PDF pages to images for Tesseract to read

Then toggle "OCR for scanned PDFs" ON in the sidebar.

---

## Languages

QuizForge comes in two versions, each as a separate executable:

| Executable | Language | Questions generated in | UI language |
|---|---|---|---|
| `QuizForge-IT.exe` | Italian | Italian | Italian |
| `QuizForge-EN.exe` | English | English | English |

### Building each version

```bash
python build.py --lang it     # builds dist/QuizForge-IT/QuizForge-IT.exe
python build.py --lang en     # builds dist/QuizForge-EN/QuizForge-EN.exe
```

You can build both and distribute them separately.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Splash screen says "Ollama installation failed" | Install Ollama manually from [ollama.com/download](https://ollama.com/download) |
| Model download stuck at 0% | Check your internet connection. The model is downloaded from Ollama's servers (~4.7 GB). |
| "No text extracted" after uploading PDF | Your PDF might be scanned. Try enabling OCR (requires Tesseract + Poppler). |
| Questions are low quality | Try "Accurate" model mode, or increase the question count. Some very short PDFs don't have enough content. |
| App doesn't close when I close the browser | Force-close from Task Manager. This is rare — normally it auto-shuts down within 20 seconds. |
| Ollama uses too much RAM | Switch to "Fast" model mode (qwen2.5:3b uses ~2 GB instead of ~5 GB). |

---

## License

MIT
