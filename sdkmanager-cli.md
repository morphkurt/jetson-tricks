# What?

Nvidia SDK Manager requires Ubuntu 18.04 or Ubuntu 16.04 with direct USB access to the Target device. Which isn't very convenient. However SDK Manager docker image can be used

```
docker run -v "/downloads:/home/nvidia/Downloads" -it --rm sdkmanager:1.7.3.9053 --cli downloadonly^C-product Jetson --version 4.6.1 --targetos Linux --target JETSON_XAVIER_NX_TARGETS --flash skip --license accept --downloadfolder /downloads
```
