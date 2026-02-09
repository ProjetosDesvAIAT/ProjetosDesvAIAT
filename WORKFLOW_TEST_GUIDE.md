# Workflow Test Guide

## Overview
This guide explains how to test the GitHub Actions workflows for generating metrics and snake contribution graph.

## Workflows

### 1. Metrics Workflow (`.github/workflows/metrics.yml`)
- **Purpose**: Generates GitHub profile metrics SVG using lowlighter/metrics
- **Output**: `github-metrics.svg` (committed to main branch)
- **Schedule**: Runs daily at 3 AM UTC
- **Manual trigger**: Yes (via workflow_dispatch)

#### How it works:
1. Checks out the repository
2. Runs lowlighter/metrics action to generate `github-metrics.svg`
3. Commits and pushes the generated SVG to the main branch

#### To test manually:
1. Go to: https://github.com/ProjetosDesvAIAT/ProjetosDesvAIAT/actions/workflows/metrics.yml
2. Click "Run workflow" button
3. Select the branch (main)
4. Click "Run workflow"
5. Wait for the workflow to complete
6. Check if `github-metrics.svg` appears in the repository root

#### Expected result:
- A new `github-metrics.svg` file should be committed to the main branch
- The README should display the metrics image at: https://raw.githubusercontent.com/ProjetosDesvAIAT/ProjetosDesvAIAT/main/github-metrics.svg

### 2. Snake Workflow (`.github/workflows/snake.yml`)
- **Purpose**: Generates contribution snake animation using Platane/snk
- **Output**: `github-contribution-grid-snake.svg` (published to output branch)
- **Schedule**: Runs daily at 2 AM UTC
- **Manual trigger**: Yes (via workflow_dispatch)

#### How it works:
1. Checks out the repository
2. Runs Platane/snk action to generate snake SVGs in `dist/` directory
3. Deploys the `dist/` directory to the `output` branch using ghaction-github-pages

#### To test manually:
1. Go to: https://github.com/ProjetosDesvAIAT/ProjetosDesvAIAT/actions/workflows/snake.yml
2. Click "Run workflow" button
3. Select the branch (main)
4. Click "Run workflow"
5. Wait for the workflow to complete
6. Check if the `output` branch was created/updated
7. Verify `github-contribution-grid-snake.svg` exists in the output branch

#### Expected result:
- A new `output` branch should be created (if it doesn't exist)
- The following files should be in the output branch:
  - `github-contribution-grid-snake.svg`
  - `github-contribution-grid-snake-dark.svg`
- The README should display the snake animation at: https://raw.githubusercontent.com/ProjetosDesvAIAT/ProjetosDesvAIAT/output/github-contribution-grid-snake.svg

## Troubleshooting

### Metrics workflow issues:
- **Problem**: SVG not generated
  - Check workflow logs for errors
  - Verify GITHUB_TOKEN has proper permissions
  
- **Problem**: SVG not committed
  - Check if the commit step succeeded
  - Verify workflow has `contents: write` permission

### Snake workflow issues:
- **Problem**: Snake not generated
  - Check if there are GitHub contributions to display
  - Verify github_user_name is correct
  
- **Problem**: Output branch not created
  - Check ghaction-github-pages step logs
  - Verify GITHUB_TOKEN permissions
  - Ensure workflow has `contents: write` permission

## Required Permissions

Both workflows require:
- `contents: write` - To commit/push changes

These permissions are already configured in the workflow files.

## Notes

- The workflows run on schedule but can also be triggered manually
- The metrics workflow commits to the main branch
- The snake workflow commits to a separate `output` branch
- Both workflows use the default GITHUB_TOKEN (no additional secrets needed)
