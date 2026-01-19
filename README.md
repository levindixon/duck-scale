# duck-scale

Generate custom "mood scale" images using AI. Takes a base image of a giant inflatable rubber duck going through 9 stages of deflation and replaces the duck with any subject you choose.

## How it works

1. A Slack workflow or external trigger calls the GitHub Actions `workflow_dispatch` API
2. The action uses OpenAI's image edit API to replace the rubber duck with your requested subject (cat, t-rex, toast, etc.)
3. The generated image is committed to the `duck-scales/` directory
4. A Slack webhook is called with the image URL

## Usage

Trigger the workflow via the GitHub API:

```bash
curl -X POST \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/OWNER/duck-scale/actions/workflows/generate-duck-scale.yml/dispatches \
  -d '{"ref":"main","inputs":{"description":"a cat","user":"username"}}'
```

## Required secrets

- `OPENAI_API_KEY` - OpenAI API key with access to the image edit API
- `SLACK_WEBHOOK_URL` - Slack workflow webhook URL

## Files

- `duck_scale.jpeg` - Base image (3x3 grid of rubber duck deflation stages)
- `duck_scale_mask.png` - Mask indicating which areas to edit (the ducks)
- `prompt_template.txt` - AI prompt template for consistent results
- `duck-scales/` - Directory where generated images are saved
