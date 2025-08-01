# For docker image

Benchmark Step:
1, docker load -i sglang.tar
2, check if loading successfully: docker images
3, docker run --name heteroflow_sglang_release --gpus all -it  --ipc=host --privileged --cap-add=SYS_NICE -v /data/DeepSeek-R1:/data/DeepSeek-R1/ sglang_release:latest
4, cd /workspace
5, sh benchmark.sh
