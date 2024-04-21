# llm

> Interact with Large Language Models (LLMs) via remote APIs and models that can be installed and run on your machine.
> More information: <https://llm.datasette.io/en/stable/help.html>.

- Set up an OpenAI API Key:

`llm keys set openai`

- Run a prompt:

`llm "{{Ten fun names for a pet pelican}}"`

- Run a [s]ystem prompt against a file:

`cat {{path/to/file.py}} | llm --system "{{Explain this code}}"`

- Install packages from PyPI into the same environment as LLM:

`llm install {{package1 package2 ...}}`

- Download and run a prompt against a [m]odel:

`llm --model {{orca-mini-3b-gguf2-q4_0}} "{{What is the capital of France?}}"`

- Create a [s]ystem prompt and [s]ave it with a template name:

`llm --system '{{You are a sentient cheesecake}}' --save {{sentient_cheesecake}}`

- Have an interactive chat with a specific [m]odel using a specific [t]emplate:

`llm chat --model {{chatgpt}} --template {{sentient_cheesecake}}`


# Custom ....................................................................................................
[Setup - LLM](https://llm.datasette.io/en/stable/setup.html)
[LLM now provides tools for working with embeddings](https://simonwillison.net/2023/Sep/4/llm-embeddings/)

## Operations
```bash
# Instlallation
pipx install llm
llm install llm-embed-jina  # jina plugin
llm embed-models  # list models

# The model will download the first time you try to use it
llm embed -m jina-embeddings-v2-small-en -c 'Hello world'

# Vimwiki
time python generate_json.py | llm embed-multi vimwiki -m jina-embeddings-v2-small-en --database embeddings.db - --format nl
llm similar vimwiki -d vimwiki.db -c 'complexity'
llm similar vimwiki -d vimwiki.db algo/algorithms.md

# bookmarks
time bkmr search --json --np | jq '[.[] | {id: .metadata, content: (.URL + ", " + .metadata + ", " + .tags)}]' | llm embed-multi bkmr -m jina-embeddings-v2-small-en --database embeddings.db --store -
llm similar bkmr -d vimwiki.db -c 'complexity'
llm similar bkmr -d vimwiki.db -c 'complexity' | jq -c -r '. | "\((.score * 10000 | round) / 10000 | tostring) \(.id) \(.content)"'
```

## Usage
[Prompt templates - LLM](https://llm.datasette.io/en/stable/templates.html)
```bash
llm 'my prompt' --key $OPENAI_API_KEY  # select keys

# continue conversion: will re-send prompts/responses for previous conversation as part of the call to the language model.
# can add up quickly in terms of tokens, especially if you are using expensive models
llm 'More names' -c

llm "Tell me about my operating system: $(uname -a)"

git diff | llm -s 'Describe these changes'  # system prompt
```

## Using System Templates
```bash
llm templates  # list templates
llm templates edit summarize  # edit
llm templates path  # show path

llm -s 'write pytest tests for this code' --save pytest
cat llm/utils.py | llm -t pytest
```

## Tools
[llm, ttok and strip-tags—CLI tools for working with ChatGPT and other LLMs](https://simonwillison.net/2023/May/18/cli-tools-for-llms/)
