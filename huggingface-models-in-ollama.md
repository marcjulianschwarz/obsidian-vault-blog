---
blog-title: Serve Hugging Face Models in Ollama
blog-tags:
  - EN
  - LLM
---

Ollama can serve many Hugging Face models locally. Concretely, you can run any [GGUF](https://github.com/ggml-org/ggml/blob/master/docs/gguf.md) model from the Hugging Face Hub. Use [this GGUF filter](https://huggingface.co/models?library=gguf) on the Hugging Face website to get a list of models that are supported (currently about 121,000 models). From the model card you can click "*Use this model*" to copy the ollama run command. For example:

```
ollama run hf.co/unsloth/DeepSeek-R1-0528-Qwen3-8B-GGUF:Q4_K_M
```

The commands are usually structured like this:

```
ollama run hf.co/{username}/{repository}:{quantization-scheme}
```

If you leave out the `quantization-scheme` part, Hugging Face will choose either `Q4_K_M` when available and a reasonable version if not.

## TypeScript Ollama Client

Given an Ollama server running on `OLLAMA_API_BASE_URL` you can use the Ollama TypeScript client to do inference on any of those models like this:

```ts
import { Ollama } from 'ollama';

const ollama = new Ollama({ host: OLLAMA_API_BASE_URL });

// Example to do inference with an embedding model
const output = await this.ollama.embed({
      model: "",
      input: text,
      truncate: true,
    });

```

See [this article on Hugging Face](https://huggingface.co/docs/hub/ollama) for more information on this.