# WSLB Action

A GitHub Action that converts Docker images into WSL (Windows Subsystem for Linux) .wsl files.

## Overview

This action leverages [wslb](https://github.com/wsl-images/wslb), a tool for building WSL images from Docker images. It automatically downloads the latest version of wslb, and converts your specified Docker image into a ready-to-import WSL distribution file. Please note it up to the docker image maintainer to ensure that the image is compatible with WSL.

## Usage

Add the following to your GitHub Actions workflow:

```yaml
- name: Build WSL image
  uses: wsl-images/wslb-action@v0.0.1
  with:
    image: ubuntu:latest      # Docker image to convert
    output_dir: ./wsl-output  # Where to save the .wsl file
```

## Inputs

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| `image` | Docker image to convert to WSL format | Yes | N/A |
| `output_dir` | Directory where the .wsl file will be saved | No | Current directory |

## Example Workflow

Below is a complete example workflow that builds a WSL image from a Docker image and verifies the created file:

```yaml
name: Build WSL File

on:
  workflow_dispatch:  # Allows manual triggering
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Create output directory
        run: mkdir -p wsl-output

      - name: Build WSL image
        uses: wsl-images/wslb-action@v0.0.1
        with:
          image: ghcr.io/wsl-images/ubuntu:latest
          output_dir: wsl-output

      - name: List created files
        run: |
          echo "Listing contents of output directory:"
          ls -la wsl-output/
          echo "Checking file type:"
          file wsl-output/* || echo "No files found or file command failed"
```

## How It Works

The action performs these steps:
1. Fetches the latest release of wslb from GitHub
2. Downloads and extracts the wslb Linux binary
3. Uses wslb to build a WSL image from your specified Docker image
4. Saves the resulting .wsl file to the specified output directory

## Requirements

- The action runs on Linux runners (ubuntu-latest recommended)
- No special permissions are required
- Internet access to download the wslb binary

## Successful Run Example

A successful run of this action can be viewed at:
https://github.com/wsl-images/wslb-action/actions/runs/13778212256

## Related Projects

- [wslb](https://github.com/wsl-images/wslb) - The command-line tool that powers this action
- [images](https://github.com/wsl-images/images) - A GitHub Action workflow that automatically builds and maintains Docker images of the official Windows Subsystem for Linux (WSL) distributions.

## License

This project is available under the MIT License.