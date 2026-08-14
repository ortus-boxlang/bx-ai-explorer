# BoxLang AI Explorer

BoxLang AI Explorer is a local, browser-based catalog of BoxLang AI examples. It presents the `.bxs` files in `samples/` by category and difficulty, with guidance, source code, and sample output for each example.

The samples cover chat, structured responses, streaming, async requests, tools, memory, agents, pipelines, RAG, orchestration, and MCP servers.

## Requirements

- macOS, Linux, or Windows with a working Java installation supported by BoxLang
- [BoxLang Version Manager (BVM)](https://boxlang.ortusbooks.com/getting-started/installation)
- An API key for the provider you want to use
- The `bx-ai` module available to your BoxLang installation

The repository pins BoxLang `1.16.0` in `.bvmrc`.

## Run Locally

From the repository root:

```sh
# Install the version pinned by this repository, if needed
bvm install 1.16.0
bvm use

# Start BoxLang MiniServer
bvm miniserver
```

Open [http://localhost:8080](http://localhost:8080) in a browser. Stop the server with `Ctrl+C`.

MiniServer reads [`miniserver.json`](miniserver.json), which serves the repository root on `localhost:8080` and uses [`config/boxlang.json`](config/boxlang.json) as the BoxLang configuration file.

To use a different port or enable debug output:

```sh
bvm miniserver --port 9090 --debug
```

Then open [http://localhost:9090](http://localhost:9090).

## Configure Provider Access

Copy the example environment file and replace the placeholder values with real credentials:

```sh
cp .env.example .env
```

Load the variables into the shell before starting MiniServer:

```sh
set -a
. ./.env
set +a
bvm miniserver
```

At minimum, configure the API key for the provider selected in `config/boxlang.json`. The default configuration uses OpenAI:

```json
"provider": "openai"
```

and the default model is `gpt-5.4-mini`. Set `OPENAI_API_KEY` in `.env` or export it directly in your shell:

```sh
export OPENAI_API_KEY="your-api-key"
```

The example environment file lists credentials for OpenAI, Anthropic/Claude, Gemini, DeepSeek, Grok, Groq, Perplexity, OpenRouter, Mistral, Hugging Face, Voyage, Cohere, and AWS providers. Only configure the providers you plan to use. Keep `.env` private and never commit API keys.

## Change the Default Provider or Model

Edit [`config/boxlang.json`](config/boxlang.json):

```json
"settings": {
    "provider": "openai",
    "defaultParams": {
        "model": "gpt-5.4-mini"
    }
}
```

Set `provider` to the provider you want to use and change `defaultParams.model` to a model supported by that provider. Individual samples can override the provider and model in their BoxLang code.

The configuration also controls:

- Request timeout with `timeout`
- Return behavior with `returnFormat` (`single`, `all`, or `raw`)
- Request and response logging with the `logRequest`, `logRequestToConsole`, `logResponse`, and `logResponseToConsole` settings
- Agent skill discovery with `skillsDirectory` and `autoLoadSkills`
- Default audio settings for speech, transcription, and translation

For a local configuration variant, set `BOXLANG_CONFIG` to another BoxLang configuration file before starting the server:

```sh
export BOXLANG_CONFIG="./config/boxlang.json"
bvm miniserver
```

## Using the Explorer

1. Start MiniServer and open the local URL.
2. Select a category or sample from the sidebar.
3. Use the search field to find samples by title or content.
4. Filter samples by difficulty.
5. Use the copy button in a sample view to copy its BoxLang source.

The explorer reads sample metadata from the JSON block at the beginning of each file. To add a sample, create a new `.bxs` file in [`samples/`](samples/) with a metadata block followed by executable BoxLang code. The filename sort order controls its position in the catalog.

## Sample Requirements

Most samples need only an AI provider key. Some examples require additional configuration:

- `014-builtin-websearch.bxs` requires a web search provider such as Brave, Tavily, or Exa. Configure the selected provider and its API key in the `bxai` settings.
- `016-file-memory.bxs` writes conversation data to the path configured by the sample. Make sure the process can write there.
- `028-document-loading.bxs` and `029-rag-system.bxs` may require local documents and an embeddings provider, respectively.
- `032-mcp-server.bxs` demonstrates an MCP server and is intended to be run as a BoxLang script rather than used as a normal chat request.

See the `guidance` metadata in each sample and the linked documentation shown in the explorer for sample-specific setup.

## Project Layout

```text
index.bxm             Explorer page and sample discovery
miniserver.json       Local MiniServer settings
config/boxlang.json   bxai provider and runtime settings
samples/              Executable BoxLang AI examples
assets/css/           Explorer styles
.env.example          Environment variable template
```

## Troubleshooting

### `bvm` or `boxlang` is not found

Install BVM, then install and select the pinned runtime:

```sh
bvm install 1.16.0
bvm use
boxlang --version
```

### The page does not load

Confirm that MiniServer is running from the repository root and that port 8080 is available. Use another port with `bvm miniserver --port 9090` if necessary.

### AI requests fail with an authentication error

Check that the environment variable for the configured provider is exported in the same shell that starts MiniServer. Also confirm that the provider name and model in `config/boxlang.json` are valid.

### The `bxai` module is missing

Install or enable the `bx-ai` module using the BoxLang module workflow, then restart MiniServer. Refer to the [BoxLang AI documentation](https://ai.ortusbooks.com/) for provider and module setup details.

## Documentation

- [BoxLang documentation](https://boxlang.ortusbooks.com/)
- [BoxLang AI documentation](https://ai.ortusbooks.com/)
- [BoxLang community](https://community.ortussolutions.com/c/boxlang/42)
