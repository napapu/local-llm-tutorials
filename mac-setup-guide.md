# Mac setup guide

Feel free to skip any steps if they don't apply to you.

## Building llama.cpp and downloading your first brain
1. Install [uv](https://docs.astral.sh/uv/#installation). 
> For downloading models but generally useful for setting up Python, which will be useful in the future.

2. Install [brew](https://brew.sh/)
> For installing build dependencies for llama.cpp

3. Create a directory for storing LLMs
    - `mkdir ~/gguf`
> See [GGUF](https://huggingface.co/docs/transformers/en/gguf) for more information. 

4. Download your first LLM from huggingface.
    - `uvx hf download unsloth/Qwen3.5-35B-A3B-GGUF Qwen3.5-35B-A3B-UD-IQ2_M.gguf --local-dir ~/gguf`
    - If that doesn't work, download directly on [huggingface site](https://huggingface.co/unsloth/Qwen3.5-35B-A3B-GGUF?show_file_info=Qwen3.5-35B-A3B-UD-IQ2_M.gguf). The **Download** button is on the right.
> This is the 'brain'. Huggingface has many of these to try out. The size of the brain is relative to how smart they are, so the download could take a while.

5. Install llama.cpp build dependencies in brew
    - `brew install cmake ninja`
> This domain moves fast, we build from source

6. Clone [llama.cpp](https://github.com/ggml-org/llama.cpp) to a known location i.e. `~/tools`
    - `git clone git@github.com:ggml-org/llama.cpp.git ~/tools/llama.cpp`
> The open-source tool for serving your local brain

7. Build it. Go to [the docs](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md) for more.
    - `cd ~/tools/llama.cpp`
    - `cmake -B build -G Ninja`
    - `cmake --build build -j`
> Step 2 should have ended with `Build files have been written to: /Users/upapan/tools/llama.cpp/build` and step 3 with `Linking CXX executable bin/llama-server`. `llama-server` is the thing you want most but there's a lot of other tools in there.

8. (Optional) Add `~/tools/llama.cpp/build/bin` to your PATH
    - `echo 'export PATH="$PATH:$HOME/tools/llama.cpp/build/bin"' >> ~/.zshrc`
    - `source ~/.zshrc`
> Makes it easier to execute from anywhere in `zsh`

9. Execute `llama-server`. You should see `main: router server is listening on http://127.0.0.1:8080`
> Validates that it built and can run successfully.

10. Serve it with the LLM.
```
llama-server \
    -m ~/gguf/Qwen3.5-35B-A3B-UD-IQ2_M.gguf \
    --ctx-size 80000 \
    --parallel 2 \
    --temp 0.6 \
    --top-p 0.95 \
    --top-k 20 \
    --min-p 0.00 \
    --cache-type-k q8_0 \
    --cache-type-v q8_0 \
    --reasoning on \
    --host 0.0.0.0 \
    --port 8001
```
 - Go to `http://localhost:8001`
- Ask it anything. You have an LLM on your machine now.
> See [this article](https://unsloth.ai/docs/models/qwen3.5) on what these values should be for this specific LLM.

> See the [README](https://github.com/ggml-org/llama.cpp/tree/master/tools/server#llamacpp-http-server) for explanation on what the parameters do.
You might find that your computer grinds to a halt, it's the way with these things.

## Next steps
1. Keep chatting, up to you.
2. Set up a coding agent
   - [Set up OpenCode](opencode-setup-guide.md) 
   - [Set up Claude Code](claude-code-setup-guide.md)