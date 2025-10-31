**Prepare Enviroment**
1. This solution uses the AMX instruction set, which is available on 4th-generation Xeon CPUs and later. Before using it, please ensure your machine has AMX instructions. You can use 'lscpu | grep amx' to check if the instruction set is available. If it is, the command's output will not be empty.
2. The base image for this image is 'nvidia/cuda:12.6.0-cudnn-devel-ubuntu24.04'. Therefore, please ensure that the CUDA version on your machine is 12.6.0 or higher and that nvidia-docker is already installed on your system.
3. The container does not include the model files, so please prepare model in advance. You can then use the 'docker -v' command to mount model into the container. Refer to the command line below for an example.  

**Benchmark Step** 

**IN BM**
1. gunzip -c sglang.tar.gz | docker load
2. docker images   
3. docker run --name sglang --gpus all -it  --ipc=host --privileged --cap-add=SYS_NICE --shm-size=16g -v /data/DeepSeek-R1:/data/DeepSeek-R1/ sglang_release:latest

**IN Docker**
1. cd /workspace  
2. sh benchmark.sh



NOTES
1. sglang.tar.gz : Subject to the specific Docker image file name received.
2. sglang_release:latest ：Base it on the ID and TAG shown by 'docker images'
3. The benchmark script sets 'OMP_NUM_THREADS=24'. Please change this value to match your machine's actual configuration.A good general guideline is to set it to your core count minus 2, and using an even number is also preferred for better performance. For example, if 'lscpu' shows BM have 48 cores per socket, you could set that value to 46.
4. When using a larger batch size, such as greater than 16, consider adding export XFT_MOE_ENGINE=0 to the benchmark script.
5. In the latest codebase, the XDNN64 parameter in the benchmark script has no impact on performance and can therefore be removed. 
