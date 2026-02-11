# 🚀 Local AI Coding Setup
Ollama + OpenCode (macOS Terminal Only)

Minimal, fast, no GUI, perfect for developers.

Tested on:
- macOS
- Apple Silicon (M1/M2/M3)
- Node/npm installed
- brew installed


--------------------------------------------------
## 1. Install Ollama (local LLM runner)
--------------------------------------------------

Install:

    brew install ollama

Test:

    ollama --version

Run server (optional, auto-starts when needed):

    ollama serve


--------------------------------------------------
## 2. Download coding models
--------------------------------------------------

Recommended (best quality/speed balance):

    ollama pull deepseek-coder:6.7b

Alternatives:

    ollama pull qwen2.5-coder:7b
    ollama pull codellama:7b

List installed models:

    ollama list

Remove a model:

    ollama rm model-name


--------------------------------------------------
## 3. Test model in terminal
--------------------------------------------------

Interactive chat:

    ollama run deepseek-coder:6.7b

Example prompt:

    write a wordpress ajax handler in php


--------------------------------------------------
## 4. Install OpenCode CLI
--------------------------------------------------

Global install:

    npm install -g opencode-ai

Test:

    opencode --help


--------------------------------------------------
## 5. Connect OpenCode to Ollama
--------------------------------------------------

Create config folder:

    mkdir -p ~/.opencode

Create file:

    nano ~/.opencode/config.json

Paste:

{
  "provider": "ollama",
  "model": "deepseek-coder:6.7b"
}

Save.


--------------------------------------------------
## 6. Use OpenCode in projects
--------------------------------------------------

Inside any project:

    opencode ask "optimize this function"
    opencode explain index.js
    opencode edit .
    opencode fix .


--------------------------------------------------
## 7. Multiple models setup (recommended)
--------------------------------------------------

Option A — change model quickly:

Edit config.json and change:

    "model": "qwen2.5-coder:7b"

Simple but manual.


--------------------------------------------------
Option B — multiple configs (better)
--------------------------------------------------

Create:

    ~/.opencode/configs/

Example:

    ~/.opencode/configs/php.json
    ~/.opencode/configs/js.json
    ~/.opencode/configs/fast.json


php.json
---------
{
  "provider": "ollama",
  "model": "deepseek-coder:6.7b"
}

js.json
---------
{
  "provider": "ollama",
  "model": "qwen2.5-coder:7b"
}

fast.json
---------
{
  "provider": "ollama",
  "model": "codellama:7b"
}


Then copy the one you want:

    cp ~/.opencode/configs/php.json ~/.opencode/config.json


--------------------------------------------------
Option C — shell aliases (BEST)
--------------------------------------------------

Edit:

    nano ~/.zshrc

Add:

alias ai="opencode ask"
alias ai-php='cp ~/.opencode/configs/php.json ~/.opencode/config.json'
alias ai-js='cp ~/.opencode/configs/js.json ~/.opencode/config.json'
alias ai-fast='cp ~/.opencode/configs/fast.json ~/.opencode/config.json'

Reload:

    source ~/.zshrc


Now you can:

    ai-php
    ai "create wordpress acf repeater loop"

    ai-js
    ai "create nuxt composable with typescript"


--------------------------------------------------
## 8. Suggested models by task
--------------------------------------------------

WordPress / PHP:
    deepseek-coder:6.7b

JavaScript / Nuxt / TS:
    qwen2.5-coder:7b

Fast/low RAM:
    codellama:7b

Heavy quality (if lots of RAM):
    deepseek-coder:33b (quantized)


--------------------------------------------------
## 9. Typical workflow
--------------------------------------------------

Chat:
    ollama run deepseek-coder:6.7b

Project help:
    ai "refactor this file"

Explain repo:
    opencode explain .

Fix bugs:
    opencode fix .

Generate code:
    ai "create express endpoint with pagination"


--------------------------------------------------
## Done 🎉
--------------------------------------------------

You now have:
✔ fully local AI
✔ no cloud
✔ no GUI
✔ fast terminal workflow
✔ multi-model support

