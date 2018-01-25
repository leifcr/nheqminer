# Yarenty version ;-)

*Changes to work on any GPU on Ubuntu - DJEZO! - (tested on CUDA 8.0  and CUDA 9.0)*

## Build instructions:

## Linux
Working solvers CPU_XENONCAT, CUDA_DJEZO
Off:CPU_TROMP, CUDA_TROMP

### General instructions:
  - Install CUDA SDK v8 (make sure you have cuda libraries in **LD_LIBRARY_PATH** and cuda toolkit bins in **PATH**)
    - example on Ubuntu:
    - LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda-8.0/lib64:/usr/local/cuda-8.0/lib64/stubs"
    - PATH="$PATH:/usr/local/cuda-8.0/"
    - PATH="$PATH:/usr/local/cuda-8.0/bin"

Dependencies:
  - Boost 1.62+ 
  - CMake v3.5 
  - Currently support only static building (CPU_XENONCAT, CUDA_DJEZO are enabled by default, check **CMakeLists.txt** in **nheqminer** root folder)
  
### Ubuntu 16.04:

NVIDIA Drivers:
```
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
```

Cuda support:
```
sudo apt-get update
sudo apt-get install cuda
sudo apt-get install cuda-toolkit-8-0
```

Tools:
```
sudo apt-get install cmake build-essential libboost-all-dev
```

Copy this repository:

`git clone https://github.com/yarenty/nheqminer.git`

Generating asm object file:
`cd nheqminer/cpu_xenoncat/asm_linux/`
`sh assemble.sh`

`cd ../../../`
`mkdir build && cd build`
`cmake ../nheqminer -DCUDA_CUDART_LIBRARY=/usr/local/cuda/lib64/libcudart.so`
`make -j $(nproc)`

*`cmake ../nheqminer -DCUDA_CUDART_LIBRARY=/usr/local/cuda-9.0/lib64/libcudart.so`*




# Run instructions:
```
Parameters: 
	-h		Print this help and quit
	-l [location]	Stratum server:port
	-u [username]	Username (bitcoinaddress)
	-a [port]	Local API port (default: 0 = do not bind)
	-d [level]	Debug print level (0 = print all, 5 = fatal only, default: 2)
	-b [hashes]	Run in benchmark mode (default: 200 iterations)

CPU settings
	-t [num_thrds]	Number of CPU threads
	-e [ext]	Force CPU ext (0 = SSE2, 1 = AVX, 2 = AVX2)

NVIDIA CUDA settings
	-ci		CUDA info
	-cd [devices]	Enable CUDA mining on spec. devices
	-cb [blocks]	Number of blocks
	-ct [tpb]	Number of threads per block
Example: -cd 0 2 -cb 12 16 -ct 64 128
```

If run without parameters, miner will start mining with 75% of available logical CPU cores. Use parameter -h to learn about available parameters:

Example to run benchmark on your CPU:

        nheqminer -b
        
Example to mine on your CPU with your own BTC address and worker1 :

        nheqminer -l eu.zec.slushpool.com:4444 -u yarenty.w1 -cd 0 
        
