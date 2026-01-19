# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Duck-scale generates custom "mood scale" images using OpenAI's image edit API. It replaces rubber ducks in a base 3x3 deflation collage with user-specified subjects (cats, dinosaurs, etc.).

## Architecture

This is a GitHub Actions-only project with no local build/test commands.

**Flow:**
1. External trigger (Slack workflow) calls GitHub Actions `workflow_dispatch` API with `description` and `user` inputs
2. Workflow cleans the description (strips "duck scale it:" prefixes)
3. Reads `prompt_template.txt` and substitutes `__DESCRIPTION__` placeholder
4. Calls OpenAI image edit API with base image, mask, and prompt
5. Commits generated image to `duck-scales/` directory
6. Calls Slack webhook with image URL, user, and description

**Key files:**
- `.github/workflows/generate-duck-scale.yml` - Main workflow
- `prompt_template.txt` - AI prompt template (edit this to change generation behavior)
- `duck_scale.jpeg` - Base image (3x3 duck deflation collage)
- `duck_scale_mask.png` - Mask defining editable regions (the ducks)

## Modifying the Prompt

Edit `prompt_template.txt` directly. Use `__DESCRIPTION__` as the placeholder for the user's subject. The prompt is designed to preserve the original collage structure while only replacing the inflatable subjects.

## Required Secrets

The workflow requires these GitHub secrets:
- `OPENAI_API_KEY` - OpenAI API key
- `SLACK_WEBHOOK_URL` - Slack workflow webhook URL
