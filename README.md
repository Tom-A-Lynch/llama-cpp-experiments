# llama.cpp RPC Setup Guide

This repository contains setup instructions for running llama.cpp with RPC support on different platforms:

- [@ubuntu-cuda/README.md](ubuntu-cuda/README.md): Guide for building and running llama.cpp with RPC support on Linux using CUDA.
- [@mac-metal/README.md](mac-metal/README.md): Guide for building and running llama.cpp with RPC support on macOS using Metal.

Each README provides detailed instructions for:

- Prerequisites
- Building the RPC server
- Building llama.cpp with RPC support
- Running distributed inference
- Best practices
- Troubleshooting
- Performance optimization
- Converting HuggingFace models to GGUF format

Refer to the specific README for your platform to get started with distributed LLM inference using llama.cpp and RPC.

> **Important:** The RPC backend is currently in a proof-of-concept stage. Use caution when deploying in production environments.
