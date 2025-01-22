This repo contains all necessary notes related to Ollama serving on Ubuntu/linux

Here are a must follow steps:

1. install the ollama library
```python
curl -fsSL https://ollama.com/install.sh | sh
```
2. verify ollama installation
```python
ollama -version
```
3. This is important to avoid later issues during running ollama as you might end-up with "No disk space Error"
    1. ```python
       mkdir -p /home/a100server/shared_folder/FFT_mnt/ollama ### this is supposed to be the folder you want to save all the models; blobs, manifest etc
    2. ```python
       mv ~/.ollama/* /home/a100server/shared_folder/FFT_mnt/ollama
    3. ```python
       ln -s /home/a100server/shared_folder/FFT_mnt/ollama ~/.ollama
    4.  ```python
        vi /etc/systemd/system/ollama.service
        ```
        [Unit]
        Description=Ollama Service
        After=network-online.target
        
        [Service]
        ExecStart=/usr/local/bin/ollama serve
        User=ollama
        Group=ollama
        Restart=always
        RestartSec=3
        Environment="PATH=/root/.cargo/bin:/root/miniconda3/envs/llama_rp/bin:/root/miniconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"
        **Environment="OLLAMA_MODELS=/home/a100server/shared_folder/FFT_mnt/ollama/models"
        Environment="OLLAMA_HOST=0.0.0.0:11434"**
        [Install]
        WantedBy=default.target

4. Now Start/restart ollama
```python
   sudo systemctl start ollama
   sudo systemctl restart ollama
```
  if you want to stop : 
  ```python 
  sudo systemctl stop ollama
  ```
5. To run ollama models;
```python
ollama run llama3.1:8b ### example to run specific model
```

6. However, if you are like me, have a custom/fine tuned model and want to run it
    1. make a modelfile (say, llama3_finetuned.modelfile) and add the path to your model
       ```python
       FROM path
    2. ```python
       ollama create llama_finetuned_model -f llama3_finetuned.modelfile
       ```

8. 
