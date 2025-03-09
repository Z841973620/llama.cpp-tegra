### 为 JetPack4.6 编译 llama.cpp，支持 Jetson Xavier/TX2/TX1/Nano

![IMG](./IMG.png)

基于 llama.cpp b3233，已修正 ggml-cuda 错误 ```identifier "__builtin_assume" is undefined```。更新的版本似乎不支持 cuda10.2

需从源码构建 gcc-8.5，默认自带的 gcc-7 缺少功能 ```vld1q_s8_x4```

编译 llama.cpp 使用 cmake-3.22.1

```
# compile gcc-8.5 from source
curl -fkLO https://bigsearcher.com/mirrors/gcc/releases/gcc-8.5.0/gcc-8.5.0.tar.gz
tar -zvxf gcc-8.5.0.tar.gz --directory=/usr/local/ && cd /usr/local/gcc-8.5.0/
./contrib/download_prerequisites
mkdir build && cd build && ../configure -enable-checking=release -enable-languages=c,c++
make && make install
```
```
# compile llama.cpp for sm_53, sm_62 and sm_72
git clone https://github.com/Z841973620/llama.cpp-tegra.git && cd llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=OFF -DLLAMA_CURL=ON -DLLAMA_CUDA=ON -DCMAKE_CUDA_ARCHITECTURES="53;62;72"
cmake --build build --config Release -j --target llama-server llama-cli
```