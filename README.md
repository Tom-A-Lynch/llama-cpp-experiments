# Building and Running llama.cpp with RPC Support on macOS

## Overview

> [!IMPORTANT]
> The RPC backend is currently in a proof-of-concept stage. It is fragile and insecure. **Never run the RPC server on an open network or in a sensitive environment!**

The RPC feature allows distributed LLM inference with `llama.cpp` by offloading computations to remote Mac Studio hosts.

## Prerequisites
- Git
- CMake
- Xcode Command Line Tools
- Metal SDK (included with macOS)

## Building RPC Server

1. Clone the repository:
   ```sh
   git clone https://github.com/ggerganov/llama.cpp.git
   cd llama.cpp
   ```

2. Build the RPC server with Metal backend:
   ```sh
   mkdir build-rpc-metal
   cd build-rpc-metal
   cmake .. -DGGML_METAL=ON -DGGML_RPC=ON
   cmake --build . --config Release
   ```

3. Start the RPC server on each Mac:
   ```sh
   ./bin/rpc-server -p 50052
   ```

## Building llama.cpp with RPC Support

1. Build llama.cpp with RPC:
   ```sh
   mkdir build-rpc
   cd build-rpc
   cmake .. -DGGML_RPC=ON
   cmake --build . --config Release
   ```

2. Run llama.cpp with RPC (replace IP addresses with your Mac IPs):
   ```sh
   ./bin/llama-cli -m ../models/tinyllama-1b/ggml-model-f16.gguf -p "Hello, my name is" --repeat-penalty 1.0 -n 64 --rpc 192.168.1.10:50052,192.168.1.11:50052,192.168.1.12:50052 -ngl 99
   ```

   Or using descriptive tags:
   ```sh
   ./bin/llama-cli -m {path_to_model} -p {prompt} --repeat-penalty {penalty_value} -n {num_tokens} --rpc {ip1}:{port1},{ip2}:{port2},{ip3}:{port3} -ngl {num_gpu_layers}
   ```

   Example for personal use with Meta-Llama-3.1-8B-Instruct model:
   ```sh
   ./bin/llama-cli -m /Volumes/PRO-G40/weights/llama-3.1/Meta-Llama-3.1-8B-Instruct/llama-3.1-model.gguf -p "Write a short story about a robot learning to paint." --repeat-penalty 1.1 -n 256 --rpc 127.0.0.1:50052 -ngl 99
   ```

## Best Practices
- Keep the repository updated:
  ```sh
  git pull origin master
  ```
- Rebuild after updates:
  ```sh
  cd build-rpc
  cmake --build . --config Release
  ```

## Useful Info
https://github.com/ggerganov/llama.cpp
https://github.com/ggerganov/llama.cpp/tree/master/examples/rpc
https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md

reach out on twitter if you have questions


