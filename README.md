# Docker with the Nvidia Container Toolkit


## Table of contents
1. [Notice](#notice)
2. [Summarized environments about the Docker with the Nvidia Container Toolkit](#env)
3. [How to install the docker](#installation_docker)
4. [How to install the NVIDIA Container Toolkit ](#installation_cnt)
5. [Docker image](#docker-image)
6. [Docker container](#docker_container)
7. [Reference](#ref)


## 1. Notice <a name="notice"></a>
- A guide for Docker with the Nvidia Container Toolkit
- I recommend that you should ignore the commented instructions with an octothorpe, #.


## 2. Summarized environments about the Docker with the Nvidia Container Toolkit <a name="envs"></a>
- Operating System (OS): Ubuntu MATE 24.04.3 LTS
- Graphics Processing Unit (GPU): NVIDIA TITAN Xp, 1ea
- GPU driver: NVIDIA-580.02.09
- CUDA toolkit: 12.8
- cuDNN: 9.13.1
- Docker: Docker version 28.5.0, build 887030f


## 3. How to install the docker <a name="installation_docker"></a>
1. Docker installation.<br />
    ```bash
    $ curl https://get.docker.com | sh
    $ sudo systemctl start docker && sudo systemctl enable docker
    ```

2. Check the docker version.<br />
    ```bash
    $ sudo docker --version
    ```
    ```bash
        Docker version 28.5.0, build 887030f
    ```


## 4. How to install the NVIDIA Container Toolkit <a name="installation_nct"></a>
1. How to install the NVIDIA Container Toolkit.<br />
    ```bash
    $ curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    $ sudo sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list
    $ sudo apt-get update
    $ export NVIDIA_CONTAINER_TOOLKIT_VERSION=1.17.8-1
    $ sudo apt-get install -y \
      nvidia-container-toolkit=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      nvidia-container-toolkit-base=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      libnvidia-container-tools=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      libnvidia-container1=${NVIDIA_CONTAINER_TOOLKIT_VERSION}
    ```

2. How to test the NVIDIA Container Toolkit.<br />
    ```bash
    $ sudo docker run --rm --gpus all nvidia/cuda:13.0.1-cudnn-devel-ubuntu24.04 nvidia-smi
    ```
    ```bash
    ==========
    == CUDA ==
    ==========

    CUDA Version 13.0.1

    Container image Copyright (c) 2016-2023, NVIDIA CORPORATION & AFFILIATES. All rights reserved.

    This container image and its contents are governed by the NVIDIA Deep Learning Container License.
    By pulling and using the container, you accept the terms and conditions of this license:
    https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license

    A copy of this license is made available in this container at /NGC-DL-CONTAINER-LICENSE for your convenience.

    Tue Oct  7 16:22:36 2025       
    +-----------------------------------------------------------------------------------------+
    | NVIDIA-SMI 580.82.09              Driver Version: 580.82.09      CUDA Version: 13.0     |
    +-----------------------------------------+------------------------+----------------------+
    | GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
    |                                         |                        |               MIG M. |
    |=========================================+========================+======================|
    |   0  NVIDIA TITAN Xp                Off |   00000000:02:00.0  On |                  N/A |
    | 23%   38C    P8             19W /  250W |     353MiB /  12288MiB |      8%      Default |
    |                                         |                        |                  N/A |
    +-----------------------------------------+------------------------+----------------------+

    +-----------------------------------------------------------------------------------------+
    | Processes:                                                                              |
    |  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
    |        ID   ID                                                               Usage      |
    |=========================================================================================|
    |  No running processes found                                                             |
    +-----------------------------------------------------------------------------------------+
    ```


## 5. Docker image <a name="docker_image"></a>
1. List docker images.
    ```bash
    usrname@hostname:~/curr_path$ sudo docker images
    ```
    ```bash
    REPOSITORY    TAG                             IMAGE ID       CREATED        SIZE
    nvidia/cuda   10.2-base                       fbd4a873bb90   7 days ago     107MB
    ```

2. Pull a docker image.
    - Target image: nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
        ```bash
        usrname@hostname:~/curr_path$ sudo docker pull nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
        ```

3. Remove a docker image.
    - Target image tag: nvidia/cuda:10.2-base
    - Target image ID: fbd4a873bb90
        ```bash
        usrname@hostname:~/curr_path$ sudo docker rmi fbd4a873bb90
        ```

4. Remove a docker image with the corresponding docker container.
    - Target image tag: nvidia/cuda:10.2-base
    - Target image ID: fbd4a873bb90
        ```bash
        usrname@hostname:~/curr_path$ sudo docker rmi -f fbd4a873bb90
        ```

## 6. Docker container <a name="docker_container"></a>
1. Run a docker container.
    - There are two options for command: i) image tag, ii) image ID.
    - Target image tag: nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
    - Target image ID: a4bdb1190443
        ```bash
        # i) image tag
        usrname@hostname:~/curr_path$ sudo docker run -it nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04 /bin/bash
        ```
        ```bash
        # ii) iamge ID
        usrname@hostname:~/curr_path$ sudo docker run -it fbd4a873bb90 /bin/bash
        ```

2. List docker containers.
    - List working docker containers
      - There are two options for result: i) image tag, ii) image ID.
          ```bash
          usrname@hostname:~/curr_path$ sudo docker ps
          ```
          ```bash
          # i) image tag
          CONTAINER ID   IMAGE                                       COMMAND       CREATED         STATUS         PORTS     NAMES
          2170c31af220   nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04   "/bin/bash"   3 minutes ago   Up 3 minutes             vigilant_feynman
          ```
          ```bash
          # ii) image ID
          CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
          f7912486a8dc   fbd4a873bb90   "/bin/bash"   5 minutes ago   Up 5 minutes             distracted_buck
          ```
    - List all docker containers
      - Please note that the results are not been shown.
      - There are two options for result: i) image tag, ii) image ID.
          ```bash
          usrname@hostname:~/curr_path$ sudo docker ps --all
          ```

3. Pause and unpause a docker container.
    - Target container ID: f7912486a8dc
    - Pause
        ```bash
        usrname@hostname:~/curr_path$ sudo docker pause f7912486a8dc
        ```
    - Unpause
        ```bash
        usrname@hostname:~/curr_path$ sudo docker unpause f7912486a8dc
        ```

4. Stop a docker container.
    - Target container ID: f7912486a8dc
        ```bash
        usrname@hostname:~/curr_path$ sudo docker stop f7912486a8dc
        ```

5. Start the stopped docker container.
    - Get the stopped docker container ID.
        ```bash
        usrname@hostname:~/curr_path$ sudo docker ps -all
        ```
        ```bash
        CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS                       PORTS     NAMES
        f7912486a8dc   fbd4a873bb90   "/bin/bash"   23 minutes ago   Exited (127) 2 minutes ago             distracted_buck
        ```
    - Target container ID: f7912486a8dc
        ```bash
        usrname@hostname:~/curr_path$ sudo docker start f7912486a8dc
        ```
    - Check whether the docker container is started.
        ```bash
        usrname@hostname:~/curr_path$ sudo docker ps
        ```
        ```bash
        CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS          PORTS     NAMES
        f7912486a8dc   fbd4a873bb90   "/bin/bash"   27 minutes ago   Up 2 seconds              distracted_buck
        ```

6. Restart the docker container.
    - Please note that the results are not been shown.
    - Target container ID: f7912486a8dc
        ```bash
        usrname@hostname:~/curr_path$ sudo docker restart f7912486a8dc
        ```

7. Attach a working docker container.
   - Target container ID: f7912486a8dc
        ```bash
        usrname@hostname:~/curr_path$ sudo docker attach f7912486a8dc
        ```
        ```bash
        root@f7912486a8dc:/#
        ```


8. Remove a docker container.
    - Please note that the docker container should be stopped before removing it.
    - Target container ID: f7912486a8dc
        ```bash
        usrname@hostname:~/curr_path$ sudo docker rm fbd4a873bb90
        ```
    - Check whether the docker container is removed.
        ```bash
        usrname@hostname:~/curr_path$ sudo docker start
        ```
        ```bash
        Error response from daemon: No such container: f7912486a8dc
        Error: failed to start containers: f7912486a8dc
        ```

9.  Remove all docker containers.
    - Please note that the docker containers should be stopped before removing them.
        ```bash
        sudo docker rm `sudo docker ps -a -q`
        ```
    - Check whether the docker containers are removed.
        ```bash
        usrname@hostname:~/curr_path$ sudo docker ps --all
        ```
        ```bash
        CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
        ```



## 7. Reference <a name="ref"></a>
1. <a href="https://www.docker.com" title="Docker"> Docker</a>.
2. <a href="https://github.com/NVIDIA/nvidia-docker" title="Nvidia-Docker"> Github for the Nvidia-Docker</a>.
3. <a href="https://docs.nvidia.com/datacenter/cloud-native" title="Nvidia-Docker"> Document for the Nvidia-Docker</a>.
