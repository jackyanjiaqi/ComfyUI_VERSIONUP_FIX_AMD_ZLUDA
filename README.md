# Running ComfyUI with AMD + ZLUDA (Windows)

## Requirements:
- ZLUDA (Install ZLUDA and add it to the PATH environment variable)
- Git
- Miniconda

1. Download and install Miniconda from [here](https://docs.anaconda.com/free/miniconda/index.html).

2. Open the Anaconda Prompt (miniconda3) terminal.

3. Create a new directory where you want to save your files.

4. Navigate to the directory and execute the following command: 
    ```bash
    conda create -n comfyui python=3.10.8 -y
    ```

5. Activate the environment:
    ```bash
    conda activate comfyui
    ```

6. Change directory to `ComfyUI`:
    ```bash
    cd .\ComfyUI\
    ```

7. Create a virtual environment:
    ```bash
    python -m venv venv
    ```

8. Activate the virtual environment:
    ```bash
    .\venv\Scripts\activate
    ```

9. Install the required packages:
    ```bash
    .\venv\Scripts\python.exe -m pip install -r .\requirements.txt
    ```

10. Install Torch and Torchvision:
    ```bash
    pip install torch==2.3.1 torchvision --index-url https://download.pytorch.org/whl/cu118
    ```

11. Navigate to `venv\Lib\site-packages\torch\lib`


12. Copy `cublas64_11.dll` and `cusparse64_11.dll` from ZLUDA (`cublas.dll` and `cusparse.dll` respectively) and paste them here.


13. 使用 `replacement/<版本号>/model_management.py` 中的文件对应你的ComfyUI版本替换，否则原版会报找不到定义的错误。
*IMPORTANT* Before running ComfyUI, you must apply the following changes in `model_management.py` located in `ComfyUI\comfy`:

    * [replacement\0.2.7\model_management.py](https://github.com/jackyanjiaqi/ComfyUI_VERSIONUP_FIX_AMD_ZLUDA/commit/f834b7df59775a6e5d56a62a6f1f145189a2ce3d)

    * 此处不需要覆盖 [cuda_malloc.py](https://github.com/jackyanjiaqi/ComfyUI_VERSIONUP_FIX_AMD_ZLUDA/blob/main/cuda_malloc.py),启动项附带参数 --disable-cuda-malloc 规避了对于此文件的替换验证。
          
14. Run the main script (规避 `cuda_malloc.py` 的未验证):
    ```bash
    python main.py --disable-cuda-malloc
    ```

Now, ComfyUI should be up and running using AMD with ZLUDA on Windows.

注意：不要使用原版的 dll文件 和 py文件直接替换，亲测一堆BUG，这个文件示例已经旧了。需要根据你的版本根据以上方法从安装的文件进行复制或者修改后复制再覆盖，只有replacement目录下的文件可以直接覆盖到对应的ComfyUI版本。该仓库拷贝自 https://github.com/zubenelakrab/ComfyUI_AMD_ZLUDA