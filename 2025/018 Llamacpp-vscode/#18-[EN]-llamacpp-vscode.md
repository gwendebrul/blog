![AI Header](images/AI-header.jpg)
## Configure LLMa.cpp in VS Code

If you don't want to use the browser for your LLMs, you can interact with them directly from Visual Studio Code. Here's how. It's a simple process.

## Requirements

You already need a working Llama.cpp on your computer. It doesn't matter if it's your work computer or another one. We'll be working with endpoints. Make sure to provide the following parameter when you start your Llama.cpp server:

    --host 0.0.0.0

You also need to have VS Code installed on your computer.

## Step 1: Install the llama-vscode extension

Start your VS Code app and search for the "llama-vscode" extension.

![Step 1](images/llamacpp-vscode-1.png)

## Step 2: Edit settings

Now let's set the endpoints. If you've installed "llama-vscode" you'll find it down in the VS Code window "llama.vscode". Click on it and choose "Edit settings". This will open a file where you can fill in the endpoints.

![Step 2](images/llamacpp-vscode-2.png)

Scroll down in the file until you see "Llama-vscode endpoints". At least fill in the endpoint for chat with the address where you can reach your Llama.cpp in the browser. For me that's "HTTP://192.168.1.243:8080".

![Step 3](images/llamacpp-vscode-3.png)

## Step 3: Open Chat

Now that you've filled in the endpoints, let's open a chat window in VS Code itself. On macOS, type  CTRL+; in an open file. On Windows, click again on "llama.vscode" at the bottom of VS Code and click "Chat with AI" or "Chat with AI with project context".

![Step 4](images/llamacpp-vscode-4.png)

## Conclusion

The chat function should now be working. Make sure your Llama.cpp server is running on the computer where you start the server. After that, you can ask questions to your AI directly from VS Code without having to leave the program.