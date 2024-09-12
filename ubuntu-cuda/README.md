# Building and Running llama.cpp with RPC Support on Linux (CUDA)

## Overview

> [!IMPORTANT]
> The RPC backend is currently in a proof-of-concept stage. It is fragile and insecure. **Never run the RPC server on an open network or in a sensitive environment!**

The RPC feature allows distributed LLM inference with `llama.cpp` by offloading computations to remote CUDA-capable hosts.

## Prerequisites
- Git
- CMake
- GCC or Clang
- CUDA Toolkit
- cuBLAS

## Building RPC Server

1. Clone the repository:
   ```sh
   git clone https://github.com/ggerganov/llama.cpp.git
   cd llama.cpp
   ```

2. Build the RPC server with CUDA backend:
   ```sh
   mkdir build-rpc-cuda
   cd build-rpc-cuda
   cmake .. -DGGML_CUDA=ON -DGGML_RPC=ON
   cmake --build . --config Release
   ```

3. Start the RPC server on each CUDA-capable machine:
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

2. Run llama.cpp with RPC (replace IP addresses with your CUDA machine IPs):
   ```sh
   ./bin/llama-cli -m ../models/tinyllama-1b/ggml-model-f16.gguf -p "Hello, my name is" --repeat-penalty 1.0 -n 64 --rpc 192.168.1.10:50052,192.168.1.11:50052,192.168.1.12:50052 -ngl 99
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
## Troubleshooting

   1. RPC connection issues: Ensure firewalls allow traffic on the specified ports.
   2. CUDA compilation errors: Verify CUDA Toolkit installation and compatibility.
   3. Out of memory errors: Adjust model size or batch size based on available GPU memory.

## Performance Optimization

   - Use `-ngl` to balance GPU layers across machines for optimal performance.
   - Experiment with different batch sizes (`--batch-size`) 
   - Quantize (`-q`) for larger models to reduce memory usage.

## Useful Info
- https://github.com/ggerganov/llama.cpp
- https://github.com/ggerganov/llama.cpp/tree/master/examples/rpc
- https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md

## Converting HuggingFace Models to GGUF Format

1. Install dependencies:
   ```sh
   pip install -r requirements.txt
   ```

2. Navigate to llama.cpp directory:
   ```sh
   cd path/to/llama.cpp
   ```

3. Run conversion script:
   ```sh
   python3 convert_hf_to_gguf.py /path/to/input/model/ --outfile /path/to/output/model.gguf
   ```

4. Use the converted model with llama.cpp

Note: Ensure sufficient disk space for conversion.