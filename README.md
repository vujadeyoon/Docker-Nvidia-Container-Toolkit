# Docker with the Nvidia Container Toolkit


## Table of contents
  1. [Notice](#notice)
  2. [Summarized environments about the Docker with the Nvidia Container Toolkit](#envs)
  3. [How to install the docker](#installation_docker)
  4. [How to install the Nvidia Container Toolkit](#installation_nct)
  5. [Docker image](#docker_image)
  6. [Docker container](#docker_container)
  7. [Reference](#ref)
  


## 1. Notice <a name="notice"></a>
- A guide for Docker with the Nvidia Container Toolkit (i.e. Nvidia-Docker 2.0)
- I recommend that you should ignore the commented instructions with an octothorpe, #.
- Modified date: Dec. 22, 2020.


## 2. Summarized environments about the Docker with the Nvidia Container Toolkit <a name="envs"></a>
- Operating System (OS): Ubuntu MATE 18.04.3 LTS (Bionic)
- Graphics Processing Unit (GPU): NVIDIA TITAN Xp, 1ea
- GPU driver: Nvidia-440.100
- CUDA toolkit: CUDA 10.2
- cuDNN: cuDNN v7.6.5
- Docker: Docker version 20.10.1, build 831ebea
- Docker image: nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04



## 3. How to install the docker <a name="installation_docker"></a>
1. Docker installation.<br />
    ```bash
    usrname@hostname:~/curr_path$ curl https://get.docker.com | sh
    usrname@hostname:~/curr_path$ sudo systemctl start docker && sudo systemctl enable docker
    ```

2. Check the docker version.<br />
    ```bash
    usrname@hostname:~/curr_path$ sudo docker --version
    ```
    ```bash
    Docker version 20.10.1, build 831ebea
    ```


## 4. How to install the Nvidia Container Toolkit <a name="installation_nct"></a>
1. Setup the repository and the Gnu Privacy Guard (GPU) key.<br />
    ```bash
    usrname@hostname:~/curr_path$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    usrname@hostname:~/curr_path$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
    usrname@hostname:~/curr_path$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
    usrname@hostname:~/curr_path$ sudo apt-get update
    ```

2. Install the Nvidia-Docker 2.0 for the Nvidia Container Toolkit.<br />
    ```bash
    usrname@hostname:~/curr_path$ sudo apt-get install -y nvidia-docker2
    usrname@hostname:~/curr_path$ sudo systemctl restart docker
    ```

3. Test the Nviida-Docker 2.0.<br />
    ```bash
    usrname@hostname:~/curr_path$ sudo docker run --rm --gpus all nvidia/cuda:10.2-base nvidia-smi
    ```
    ```bash
    Tue Dec 22 08:40:19 2020
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 440.100      Driver Version: 440.100      CUDA Version: 10.2     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  TITAN Xp            Off  | 00000000:01:00.0  On |                  N/A |
    | 23%   30C    P8    10W / 250W |    518MiB / 12192MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
                                                                                
    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    +-----------------------------------------------------------------------------+
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
