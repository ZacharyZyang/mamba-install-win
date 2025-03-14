# mamba-install-win
mamba2安装（Windows）
# 前期准备
0. visual studio安装，2022 community版本即可，安装时选中windows sdk和MSVC v143 - VS 2022 C++ x64/x86生成工具，安装完毕配置环境变量；

1. cmake安装，下载地址 https://cmake.org/download/ ，选择 windows x64 zip即可，然后配置环境变量，Path添加对应地址如D:\Program Files\CMake\bin即可；

2. ninja安装：下载地址 https://github.com/ninja-build/ninja/releases ，选择ninja-win.zip即可，这就是一个.exe文件，将ninja.exe文件所在文件夹地址添加到环境变量Path中即可；参考链接https://blog.csdn.net/HaoZiHuang/article/details/126083356 ；

3. cuda toolkit安装，下载地址 https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64 ，选择windows系统x86_64和对应版本10或11，然后下载安装即可，并添加系统环境变量路径。与下面对应，可以安装12.4.0版本。

4. 创建conda虚拟环境安装torch，此处使用torch版本2.4.1，cuda版本12.4.0

conda create -n mamba python=3.10
conda activate mamba

pip install torch==2.4.1 torchvision==0.19.1 torchaudio==2.4.1 --index-url https://download.pytorch.org/whl/cu124

python -c "import torch; print(torch.cuda.is_available())" # 验证torch安装

conda install nvidia/label/cuda-12.4.0::cuda-nvcc

pip install setuptools==68.2.2
conda install packaging

5. 安装trition-windows
参考博客 https://blog.csdn.net/yyywxk/article/details/144868136 ，需要准备MSVC和windows sdk，这在前面第0步安装vs时解决；
安装命令 pip install https://github.com/woct0rdho/triton-windows/releases/download/v3.1.0-windows.post5/triton-3.1.0-cp310-cp310-win_amd64.whl
也可先下载在安装；安装完毕可验证其核心的triton.jit 和 torch.compile等功能。

# 安装causal-conv1d
6. 从源码编译安装causal-conv1d
参考 https://blog.csdn.net/yyywxk/article/details/143924743 进行源码修改，
同时setup.py文件中第37行FORCE_BUILD = os.getenv("CAUSAL_CONV1D_FORCE_BUILD", "FALSE") == "TRUE"，将其中的FALSE改为TRUE。
同时将165-171行注释掉，以及185行注释掉，避免出现报错63行release_idx = output.index("release") + 1报错release。
最后安装时用命令python setup.py install

# 安装mamba-ssm
7. 从源码安装mamba-ssm
参考https://blog.csdn.net/yyywxk/article/details/140420538 进行源码修改，可以安装最新版，不用git checkout v1.1.3来改变分支
同时用python setup.py install命令安装，如果出现相关error在该博客中可以找到issue解决方法。
如果出现“kNRows_”相关错误，参考该链接 https://github.com/state-spaces/mamba/issues/12#issuecomment-1848835662 进行修改。
