# Quickstart: LMCache Huge Data Access Tutorial

## Prerequisites
1. **GPU Environment**: A Linux machine with NVIDIA GPU and CUDA 12.1+.
2. **LMCache & vLLM**: Ensure `lmcache` and `vllm` are installed.
3. **LangFuse**: An API key for LangFuse (Cloud or local instance).

## Setup
1. Clone the repository.
2. Install dependencies:
   ```bash
   pip install vllm lmcache langgraph langfuse jupyter pandas matplotlib
   ```
3. Start the vLLM server with LMCache:
   ```bash
   python -m vllm.entrypoints.openai.api_server 
       --model <model_path> 
       --enable-prefix-caching 
       --host 0.0.0.0 
       --port 8000
   ```

## Running the Tutorial
1. Launch Jupyter:
   ```bash
   jupyter notebook notebooks/lmcache_huge_data_tutorial.ipynb
   ```
2. Follow the cells in order:
   - **Cell 1**: Initialize LangFuse and vLLM client.
   - **Cell 2**: Load the large "Manual" text.
   - **Cell 3**: Define the LangGraph workflow.
   - **Cell 4**: Run "Cold Start" inference (Expect high TTFT).
   - **Cell 5**: Run "Cache Hit" inference (Expect low TTFT).
   - **Cell 6**: Run "Cache Busted" inference (Modify prefix, expect high TTFT).
   - **Cell 7**: Visualize results and check LangFuse dashboard.
