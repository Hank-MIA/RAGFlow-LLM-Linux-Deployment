# RAGFlow + Ollama on WSL2

### Use Ollama to serve LLM
1. Download Ollama
    - https://ollama.com/download/linux
    - `curl -fsSL https://ollama.com/install.sh | sh`
2. Run the LLM of your choice
    - https://ollama.com/search
    - `ollama run deepseek-r1:32b`
    - This will download the LLM and start to run it. Your model is saved in `.ollama/models`. `du -h` to check size of the directory.
    - You should be able to chat with your LLM now. :joy: `ctrl + D` to exit.
3. By default Ollama only serves (listen to) localhost:11434. Ragflow that runs in docker container will be considered as a different network. So update the Ollama's config to serve on 0.0.0.0:11434:
    - `sudo nano /etc/systemd/system/ollama.service`
    [Service]
    Environment="OLLAMA_HOST=0.0.0.0:11434"