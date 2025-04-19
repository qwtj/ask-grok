# ask-grok

This script allows you to interact with the Grok API from the command line.  It supports specifying a prompt, attaching a file, choosing a model, and displaying the output using glow (if installed).

## Prerequisites

*   **jq:**  A lightweight and flexible command-line JSON processor.
    *   Install on macOS: `brew install jq`
    *   Install on Debian/Ubuntu: `sudo apt-get install jq`
*   **curl:** A command-line tool for transferring data with URLs.  Most systems have this already.
*   **glow (optional):**  A markdown renderer.  If installed, the output will be displayed formatted.
    *   Install on macOS: `brew install glow`
    *   Install via Go: `go install github.com/charmbracelet/glow@latest`
*   **zsh:** The script is written in zsh.
*   **XAI_API_KEY:** You will need an API key from x.ai.  Set this as an environment variable.

## Installation

1.  Download the script (e.g., `ask-grok.zsh`) and make it executable:

    ```bash
    wget <link_to_raw_script> -O ask-grok.zsh
    chmod +x ask-grok.zsh
    ```

2.  Set the `XAI_API_KEY` environment variable.  You can add this to your `.zshrc` or `.bashrc` file:

    ```bash
    export XAI_API_KEY="your_api_key_here"
    ```

    Remember to source your shell configuration file after making changes (e.g., `source ~/.zshrc`).

## Usage

```bash
./ask-grok.zsh [options] <prompt>
```

### Options

*   `<prompt>`: The text prompt to send to the Grok API. This can be passed directly on the command line, via a file with `-p`, or via standard input.  If both are provided, the command-line prompt is appended to the file content and then to the standard input.
*   `-p <prompt path>`: Provide a path to a file containing the prompt.
*   `-v`: Enable verbose mode, which displays more detailed information about the request and response.
*   `-h, --help`: Display the help message.
*   `-m <model name>`: Specify the model to use (default: `grok-3`).  Use `-i` to list available models.
*   `-g`: Use glow to display the output, rendering it as markdown.
*   `-f <attached_File>`: Path to a file to include in the request.  The file's contents will be appended to the prompt.
*   `-i`: List available models and exit. This requires a valid `XAI_API_KEY`.

### Examples

*   **Simple prompt:**

    ```bash
    ./ask-grok.zsh "What is the capital of France?"
    ```

*   **Prompt with glow:**

    ```bash
    ./ask-grok.zsh -g "Explain the concept of quantum entanglement."
    ```

*   **Specify a model:**

    ```bash
    ./ask-grok.zsh -m grok-3 "Write a short poem about autumn."
    ```

*   **Prompt from a file:**

    ```bash
    ./ask-grok.zsh -p prompt.txt
    ```

*   **Attach a file:**

    ```bash
    ./ask-grok.zsh -f code.py "Analyze the following Python code."
    ```

*   **List available models:**

    ```bash
    ./ask-grok.zsh -i
    ```

*   **Prompt via standard input:**

    ```bash
    cat prompt.txt | ./ask-grok.zsh
    ```

*   **Combination of file, stdin, and command line prompt:**
    ```bash
    cat stdin.txt | ./ask-grok.zsh -p prompt.txt "And summarize all of this."
    ```
    In this case, the content of `prompt.txt` will be the primary prompt, the content of `stdin.txt` will be appended, and the final command line argument "And summarize all of this." will be appended after.

### Environment Variables

*   `XAI_API_KEY`:  Your Grok API key.  This is required for the script to function.

## Error Handling

The script includes error handling for:

*   Missing or invalid `XAI_API_KEY`.
*   Missing or invalid file paths.
*   Invalid JSON in the request or response.
*   Empty or missing prompts.
*   Invalid command-line options.

Verbose mode (`-v`) can be useful for debugging issues.

## Notes

*   The script removes temporary files created for the request body.  In verbose mode, it retains the request and response files for debugging.
*   The script attempts to clean and escape the prompt content to ensure it is valid JSON.  This includes removing control characters and escaping special characters.
*   The script uses `base64` encoding to pass the response from the Grok API to the output.  This helps prevent issues with character encoding.
*   The `list_available_models` function tries to query the Grok API for a list of available models.  However, this API endpoint may not always be available.  In this case, the function will fall back to a hardcoded list of models. It is recommended to check the official Grok API documentation for the most up-to-date list of models.

## License

This script is provided as-is and is released under the MIT License. See the LICENSE file for details.
