
# llama-cpp-python - MacOS Install with Metal GPU


**(1) Make sure you have xcode installed... at least the command line parts**
```
# check the path of your xcode install 
xcode-select -p

# xcode installed returns
# /Applications/Xcode-beta.app/Contents/Developer

# if xcode is missing then install it... it takes ages;
xcode-select --install
```

**(2) Install the conda version for MacOS that supports Metal GPU**
```
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh
bash Miniforge3-MacOSX-arm64.sh
```

**(3) Make a conda environment**
```
conda create -n llama python=3.9.16
conda activate llama
```

**(4) Install the LATEST llama-cpp-python.. which, as of just today, happily supports MacOS Metal GPU**  
    *(you needed xcode installed in order pip to build/compile the C++ code)*
```
pip uninstall llama-cpp-python -y
CMAKE_ARGS="-DLLAMA_METAL=on" FORCE_CMAKE=1 pip install -U llama-cpp-python --no-cache-dir
pip install 'llama-cpp-python[server]'

# you should now have llama-cpp-python v0.1.62 installed
llama-cpp-python         0.1.62      

```

**(4) Download a v3 ggml llama/vicuna/alpaca model**
 - **ggmlv3**
 - file name ends with **q4_0.bin** - indicating it is 4bit quantized, with quantisation method 0

https://huggingface.co/vicuna/ggml-vicuna-13b-1.1/blob/main/ggml-vic13b-q4_0.bin
https://huggingface.co/vicuna/ggml-vicuna-13b-1.1/blob/main/ggml-vic13b-uncensored-q4_0.bin
https://huggingface.co/TheBloke/LLaMa-7B-GGML/blob/main/llama-7b.ggmlv3.q4_0.bin
https://huggingface.co/TheBloke/LLaMa-13B-GGML/blob/main/llama-13b.ggmlv3.q4_0.bin


**(6) run the llama-cpp-python API server with MacOS Metal GPU support**
```
# config your ggml model path
# make sure it is ggml v3
# make sure it is q4_0
export MODEL=[path to your llama.cpp ggml models]]/[ggml-model-name]]q4_0.bin
python3 -m llama_cpp.server --model $MODEL  --n_gpu_layers 1
```

***Note:** If you omit the `--n_gpu_layers 1` then CPU will be used*


