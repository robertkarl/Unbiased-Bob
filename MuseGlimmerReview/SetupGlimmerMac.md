This is a prompt intended to be fed to a frontier-level agent running on a Mac. Brought to you by Unbiased-Bob on YouTube.

If you're not at the intelligence level of Opus or GPT Sol, your mileage may vary.

# setup
The user wants to run a recently (as of August 2026) released model from Meta called Muse Glimmer.

* If this mac has less than 32GB of RAM; give up now and report that the hardware doesn't support it.
* if on Linux; check that nvidia-smi works and there is a graphics card with 24GB of RAM or greater.

Check disk free too; if you don't have 20GB or more free on '/', how do you even get by?

Desired outcome / success criteria:

- llama-server is built from source and placed in ~/bin/llama-server
- Don't set it up to be run at launch
- Add a script to ~/bin/ called start-llama-muse-spark.sh
- The user has been notified that the files have been placed there
- The muse spark Q4 model from unsloth has been downloaded 
- the dflash speculative head has been downloaded

Desired flags: (Apple Silicon or Nvidia targets)

Replace the model path.

```
  --model /mnt/models/Muse-Glimmer-30B-UD-Q4_K_XL.gguf \
  --model-draft /mnt/models/dflash-kquant.gguf \
  --spec-type draft-dflash --spec-draft-n-max 2 \
  -ngl 99 -fa on -lv 4 \
  --cache-type-k q4_0 --cache-type-v q4_0 \
  --ctx-size 131072 --parallel 2 --kv-unified \
  --cache-ram 4096 \
  --reasoning on --reasoning-budget 8192 \
  --temp 1.0 --top-p 0.95 --top-k 64 --min-p 0 \
  --presence-penalty 0
```


# runbook
Ask the user if they want to download  the following into ~/bin/llama-glimmer/

* the model https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF?show_file_info=Muse-Glimmer-30B-UD-Q4_K_XL.gguf
* the speculative head https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF/blob/main/dflash-kquant.gguf
* the vision projection https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF/blob/main/mmproj-kquant.gguf
* the source code for llama-server https://github.com/ggml-org/llama.cpp/tree/master. Releases are done by tags prefixed with 'b' like https://github.com/ggml-org/llama.cpp/releases/tag/b10380. Grab a recent one. the one I just linked supports Glimmer.

Downloading. use the 'hf' binary if installed/possible. it's faster and has a nice cache. otherwise curl or whatever works.

Create a simple 'run' script that passes the right flags to llama-server.

After getting approval; make the directory and download the requisite files.

Then build llama-server. it takes a long time on Nvidia and is quite fast on Apple Silicon.

# verification
- run the binary and hit the server with some sample requests. Expect >= 1000 prompt processing speed on nvidia and 200-250 tokens/s prompt processing on Apple Silicon.
- Expect 50-70 tokens/second decode on Nvidia. expect lower on Mac.
- If Nvidia and significantly lower? Raise this as an error to the user.

# if everything worked

Inform the user of success; and ask if they want to add ~/bin/llama-glimmer/ to their zsh path.

Inform user how to run the script to start the server.
