# Ollama cheatsheet

## Install
Run the below line which will download and install the tool on Mac

```
curl -fsSL https://ollama.com/install.sh | sh
```

## Delete ollama from the system
Run all these commands

```
sudo rm -rf /Applications/Ollama.app
sudo rm /usr/local/bin/ollama
rm -rf "~/Library/Application Support/Ollama"
rm -rf "~/Library/Saved Application State/com.electron.ollama.savedState"
rm -rf ~/Library/Caches/com.electron.ollama/
rm -rf ~/Library/Caches/ollama
rm -rf ~/Library/WebKit/com.electron.ollama
rm -rf ~/.ollama
```

## Basic commands

### Run a model

```
ollama run <model name>
```

This looks for model in ~/.ollama/models and if not found, it first downloads the model from ollama server. Once downloaded, it is loaded into memory and a cli chat window is presented in terminal. To close the chat window, type

```
/bye
```

This closes the chat but the model is still running in the local ollama server. To verify this, open any browser and type

```
http://localhost:11434/
```

If the server is running, you should see a message

```
Ollama is running
```

### Download a new model
```
ollama pull <model name>
``` 

This downloads a new model to ~/.ollama/models. This always downloads from ollama website.

To download from Huggingface, use this format

```
ollama pull hf.co/`<username>`/`<repository>`:`<quant-tag>`
```

For example

```
ollama pull hf.co/Qwen/Qwen2.5-Coder-3B-Instruct-GGUF:q4_k_m
```

### List of models downloaded on your local system

```
ollama ls
```

sample output
```
NAME                        ID              SIZE      MODIFIED     
deepseek-coder:1.3b-base    3b417b786925    776 MB    2 hours ago     
stable-code:3b              37681d29a55a    1.6 GB    2 hours ago     
qwen2.5-coder:3b-base       b71a053ff1b3    1.9 GB    13 hours ago    
qwen2.5-coder:7b-base       bd8755145f1c    4.7 GB    13 hours ago    
qwen2.5-coder:1.5b          d7372fd82851    986 MB    14 hours ago    
codellama:13b-code          61b6aa1b3d0f    7.4 GB    15 hours ago    
llama3.1:latest             46e0c10c039e    4.9 GB    16 hours ago    
qwen:latest                 d53d04290064    2.3 GB    16 hours ago    
qwen2.5-coder:7b            dae161e27b0e    4.7 GB    4 days ago 
```

### List of models currently loaded into memory

```
ollama ps
```

sample output
```
NAME                     ID              SIZE      PROCESSOR    CONTEXT    UNTIL               
qwen2.5-coder:3b-base    b71a053ff1b3    2.4 GB    100% GPU     8192       29 minutes from now    
qwen2.5-coder:7b         dae161e27b0e    4.7 GB    100% GPU     4096       4 minutes from now
```

### Stop a running model

```
ollama stop <model name>
```

### Delete a model

```
ollama rm <model name>
```

### Getting detail of a model

```
ollama show <model name>
```

Sample output
```
Model
    architecture        qwen2     
    parameters          3.1B      
    context length      32768     
    embedding length    2048      
    quantization        Q4_K_M    

  Capabilities
    completion    
    insert        

  License
    Qwen RESEARCH LICENSE AGREEMENT                                     
    Qwen RESEARCH LICENSE AGREEMENT Release Date: September 19, 2024    
    ...         
```

Things to note here:
- In capabilities, if completion is there, it can predict next word from previous words. So in the current writing stream, it predicts the next stream of words.
- If insert is there, it can generate text at any random location. Say you are writing something and realised there is a mistake few paragraphs before, you can use this model to correct the mistakes.
- If tools is present, then it can parse payloads like json and use external tools like APIs to connect to databases or other services.


## Close the ollama server

In order to do this, we need to kill the ollama server from ps and kill -9 commands. This server doesn't take up much resource on its own, so keeping it running is not an issue. We can simply unload all the models from memory to free up system resources.
