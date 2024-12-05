# vllm-usecases

## 1. Chat application

### Steps to run a chatbot using vLLM as backend
This application is run from a docker container

Prerequisites:
- vLLM image ( can be built from [here](https://github.com/vllm-project/vllm/blob/main/Dockerfile.ppc64le))
- HuggingFace token pertaining to the model you want to run (only needed for gated models)

1. export HF token
    ```bash
    export HF_TOKEN=<>
    ```
2. Run your container in detached mode , with the following variables
    ```bash
    docker run -itd --entrypoint /bin/bash -v ~/.cache/huggingface:/root/.cache/huggingface --privileged=true --network host -e HF_TOKEN=$HF_TOKEN --cpus <> -m <>GB --name <vllm-image> cpu-test 
    ```
3. Copy the script to run the client application in the container
    ```bash
    docker cp streamlit-chat-app.py  cpu-test:vllm/examples/ 
    ```

4. Run your vLLM server with desired model and your chat client
    ```bash
    docker exec cpu-test bash -c "
        /opt/conda/bin/python3 -m vllm.entrypoints.openai.api_server --model meta-llama/Llama-3.1-8B-Instruct --dtype half --max-model-len 4000
        pip install streamlit
        streamlit run vllm/examples/streamlit-chat-app.py" 
    ```
5. Access your chatbot on this link in your browser
    ```bash
    http://<ip of host vm where container is running>:8501 
    ```


