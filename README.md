# RAGFlow + Ollama on WSL2

## 1. Use Ollama to serve LLM
1. **Download Ollama** from https://ollama.com/download/linux
   ```bash
   curl -fsSL https://ollama.com/install.sh | sh
   ```

2. **Run the LLM of your choice** https://ollama.com/search
    - Use deepseek-r1:32b as an example
   ```bash
   ollama run deepseek-r1:32b
   ```
    - This will download the LLM and start to run it. Your model is saved in `.ollama/models`. `du -h` to check size of the directory.
    - **You should be able to chat with your LLM now. :smiley: `ctrl + D` to exit**.

3. By default Ollama only serves (listen to) localhost:11434. Ragflow that runs in docker container will be considered as a different network. So **update the Ollama's config to serve on 0.0.0.0:11434**:
   ```bash
   sudo nano /etc/systemd/system/ollama.service
   ```
   ```
   [Service]
   Environment="OLLAMA_HOST=0.0.0.0:11434"
   ```

## 2. Install Docker
1.  **Update your package information:**
    ```bash
    sudo apt update
    ```

2.  **Install packages to allow apt to use HTTPS repositories:**
    ```bash
    sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
    ```

3.  **Add Docker's official GPG key:**
    ```bash
    curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```

4.  **Add the Docker repository:**
    ```bash
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

5.  **Update package database with Docker packages:**
    ```bash
    sudo apt update
    ```

6.  **Install Docker Engine, CLI, containerd, Buildx plugin, and Compose plugin:**
    ```bash
    sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

7.  **Add your user to the docker group to run Docker without sudo:**
    ```bash
    sudo usermod -aG docker $USER
    ```
    ```bash
    sudo shutdown now
    ```
    Then start WSL again.

8.  **Start the Docker service:**
    ```bash
    sudo service docker start
    ```

9.  **Verify Docker is installed correctly:**
    ```bash
    docker run hello-world
    ```
    This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.

## 3. Clone Ragflow repo, compose and run Ragflow image
1. Set the maximum number of memory map areas a process can have. (This might be optional)
   ```
   C:\Users\user_name\.wslconfig
   ```
   ```
   [kernel]
   vm.max_map_count=262144
   ```
   ```bash
   sudo shutdown now
   ```
   Then start WSL again.

2. **Clone Ragflow repo**
   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   ```

3. **Select Image with Embedding Models (none slim version)**
   ```bash
   cd ragflow/docker
   nano .env
   ```
   ```
   # RAGFLOW_IMAGE=infiniflow/ragflow:v0.18.0-slim
   RAGFLOW_IMAGE=infiniflow/ragflow:v0.18.0
   ```

4. **Start up the server using the pre-built Docker images:**
   ```bash
   # Use CPU for embedding and DeepDoc tasks:
   docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate embedding and DeepDoc tasks:
   # docker compose -f docker-compose-gpu.yml up -d
   ```

## 4. Create AI assistant with private knowledge base!
    - Video demo comming soon!

## 5. Properly shutdown
   - If the containers are not properly shutdown, it may cause error next time 
   ```bash
   docker compose down
   ```