# DEPRECATED

There is now an [official action](https://github.com/anthropics/claude-code-action) so this repo is no longer needed or maintained. It probably doesn't work and shouldn't be used.

# Claude Code Action

A GitHub Action for running [Claude Code](https://github.com/anthropics/claude-code) on your repository.

## Description

This action allows you to run Claude Code in your GitHub Actions workflow. Claude Code is an agentic coding tool from Anthropic that understands your codebase, and helps you code through natural language commands.

## Prerequisites

- An Anthropic API key

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `prompt` | The prompt to send to Claude Code | Yes |
| `acknowledge-dangerously-skip-permissions-responsibility` | Set to "true" to acknowledge that you have read and agreed to the disclaimer shown when running `claude code --dangerously-skip-permissions` | Yes |

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `ANTHROPIC_API_KEY` | Your Anthropic API key | Yes |

## Outputs

| Output | Description |
|--------|-------------|
| `result` | The output from Claude Code |

## Usage

```yaml
name: Run Claude Code

on:
  workflow_dispatch:
    inputs:
      prompt:
        description: 'Prompt for Claude Code'
        required: true
        type: string

jobs:
  run-claude-code:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Run Claude Code
        id: claude
        uses: joesarre/claude-code-action@v1
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        with:
          prompt: ${{ github.event.inputs.prompt }}
          acknowledge-dangerously-skip-permissions-responsibility: "true"
          
      - name: Use Claude's output
        run: |
          echo "Claude's output: ${{ steps.claude.outputs.result }}"
```

## About the `--dangerously-skip-permissions` Flag

This action uses the `--dangerously-skip-permissions` flag with Claude Code. This flag allows Claude to modify files in your repository without asking for permission for each change. By setting `acknowledge-dangerously-skip-permissions-responsibility` to "true", you acknowledge that you understand the risks associated with this flag.

## Output

The action will modify the files in the working directory of the calling workflow and will output a summary from Claude Code to the workflow logs. The output is also available as the `result` output variable.

## License

See the [LICENSE](LICENSE) file for details.
