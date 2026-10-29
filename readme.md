# PyTorch 2.3.1 CUDA 12.9 适配补丁

本仓库提供 PyTorch v2.3.1 在 CUDA 12.9 / Blackwell (sm_120) 环境下源码编译所需的补丁。
所有修改均在 **BSD-3-Clause** 许可证下分发，原始版权归 PyTorch 贡献者所有。

## 修改内容

1. **NVTX v3 适配** (`cmake/public/cuda.cmake`)
   - CUDA 12.9 移除了 `libnvToolsExt.so`，改为 header-only 的 NVTX v3
   - 移除对 `CUDA::nvToolsExt` 目标的强制检查，改用 `nvtx3` 头文件
   - 全局定义 `TORCH_CUDA_USE_NVTX3` 宏

2. **源码 NVTX 头文件切换** (`torch/csrc/profiler/stubs/cuda.cpp`, `torch/csrc/cuda/shared/nvtx.cpp`)
   - 条件编译：定义 `TORCH_CUDA_USE_NVTX3` 时包含 `<nvtx3/nvToolsExt.h>`

3. **支持 sm_120** (`torch/utils/cpp_extension.py`)
   - 在 `supported_sm` 列表中加入 `12.0`，使扩展编译能识别 Blackwell 架构


## 使用方法

### 方法一：apply patch

```bash
git clone --branch v2.3.1 https://github.com/pytorch/pytorch.git
cd pytorch
git am /path/to/*.patch
python setup.py bdist_wheel
```

### 方法二：直接使用我构建的 wheel
 https://pan.baidu.com/s/14O96snHNEu1wjz8XL3nWAQ?pwd=ag4h
 
  提取码: ag4h

### 环境
* CUDA 12.9
* Python 3.8
* GCC 13.3.0（注意：高版本 GCC 可能有其它兼容性问题）
* TORCH_CUDA_ARCH_LIST=12.0（或按你的 GPU 设置）

### 免责声明
这些补丁仅用于本地调试/研究。官方 PyTorch 2.3.1 发布时并不支持 CUDA 12.9 与 Blackwell 架构，
正式项目建议使用官方支持的新版本 PyTorch