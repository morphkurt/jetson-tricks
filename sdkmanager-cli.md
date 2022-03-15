# What?

Nvidia SDK Manager requires Ubuntu 18.04 or Ubuntu 16.04 with direct USB access to the Target device. Which isn't very convenient. However SDK Manager docker image can be used

```
docker run -v "/downloads:/home/nvidia/Downloads" -it --rm sdkmanager:1.7.3.9053 \
--cli downloadonly^C-product Jetson --version 4.6.1 --targetos Linux \
--target JETSON_XAVIER_NX_TARGETS --flash skip --license accept --downloadfolder /downloads
```

After downloading the file look at the sdkml3_jetpack_461.json. Which can be used to figure out the dependcies for SDK manager.

# For SVA the following is required

## Cuda

```
# install the cuda repo
dpkg -i cuda-repo-l4t-10-2-local_10.2.460-1_arm64.deb
apt install cuda-toolkit-10-2
```

## cuDNN on Target

```
cat <<EOF > cuDNN-dep
libcudnn8_8.2.1.32-1+cuda10.2_arm64.deb
libcudnn8-dev_8.2.1.32-1+cuda10.2_arm64.deb
EOF
for i in `cat cuDNN-dep` ;do dpkg -i $i ;done
```

## TensorRT on Target

```
cat <<EOF > TensorRT-dep
libnvinfer8_8.2.1-1+cuda10.2_arm64.deb
libnvinfer-dev_8.2.1-1+cuda10.2_arm64.deb
libnvinfer-plugin8_8.2.1-1+cuda10.2_arm64.deb
libnvinfer-plugin-dev_8.2.1-1+cuda10.2_arm64.deb
libnvonnxparsers8_8.2.1-1+cuda10.2_arm64.deb
libnvonnxparsers-dev_8.2.1-1+cuda10.2_arm64.deb
libnvparsers8_8.2.1-1+cuda10.2_arm64.deb
libnvparsers-dev_8.2.1-1+cuda10.2_arm64.deb
libnvinfer-bin_8.2.1-1+cuda10.2_arm64.deb
libnvinfer-doc_8.2.1-1+cuda10.2_all.deb
libnvinfer-samples_8.2.1-1+cuda10.2_all.deb
tensorrt_8.2.1.8-1+cuda10.2_arm64.deb
python3-libnvinfer_8.2.1-1+cuda10.2_arm64.deb
python3-libnvinfer-dev_8.2.1-1+cuda10.2_arm64.deb
graphsurgeon-tf_8.2.1-1+cuda10.2_arm64.deb
uff-converter-tf_8.2.1-1+cuda10.2_arm64.deb

EOF
for i in `cat TensorRT-dep` ;do dpkg -i $i ;done
```

## Nvidia Docker on Target

```
cat <<EOF > nvidiaDocker-dep
nvidia-container-csv-cuda_10.2.460-1_arm64.deb
nvidia-container-csv-cudnn_8.2.1.32-1+cuda10.2_arm64.deb
nvidia-container-csv-tensorrt_8.2.1.8-1+cuda10.2_arm64.deb
libnvidia-container0_0.10.0+jetpack_arm64.deb
libnvidia-container1_1.7.0-1_arm64.deb
libnvidia-container-tools_1.7.0-1_arm64.deb
nvidia-container-toolkit_1.7.0-1_arm64.deb
nvidia-container-runtime_3.7.0-1_all.deb
nvidia-docker2_2.8.0-1_all.deb
EOF
for i in `cat nvidiaDocker-dep` ;do dpkg -i $i ;done
```
