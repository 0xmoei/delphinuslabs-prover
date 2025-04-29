# DelphinusLabs Prover
A step by step guide on running delphinus zkWASM prover and earn credits.

[Official Prover Repository
](https://github.com/DelphinusLab/prover-node-docker/tree/main?tab=readme-ov-file#prover-node-configuration)
## Requirement
* GPU: min. RTX 4090
* RAM: >82 GB
* Nvidia CUDA, Docker
* **Local GPU PC** Or **Cloud GPU with `VM ubuntu` template**
* Ubuntu ( Windows users can [Install Ubuntu](https://github.com/0xmoei/Install-Linux-on-Windows) too)

## Rewards
* TGE in Q4. Provers based on their performance share 3% of token allocation at TGE.
* If the `liveness-time` is `90%+` and your proving speed keeps stable, then there will be around `estimated $100+/month` + `base` * `number of proofs` you have generated.
* From the current estimate of the overall provers, the **minimal reward** of a **certified** node is around `$200/month`.
the maximal is `$380`.

![image](https://github.com/user-attachments/assets/c9c17d92-0bf9-4095-9bf3-7bd6d0623841)


## Notes
* The most profittable way is to use `Local PC` GPUs.
* I have tested it on a cloud GPU: `RTX 4090 - 24GB vRAM` **($0.41/hr in [Vast.ai](https://cloud.vast.ai/?ref_id=228875))** and it's so fast with 30 tasks/day.
* In my case, it might even NOT be profitable at TGE, I just love experiment.
* Team says only supports `4090` but you might need to test with lower GPUs if you want. I believe `24GB vRAM` is 100% needed.
* If you run on `Cloud GPU ` like [Vast.ai](https://cloud.vast.ai/?ref_id=228875), you need to install a `VM Ubuntu` template. and NOT `CUDA` or `Pytorch`.
* Since the Prover installation is using Docker, then `CUDA` or `Pytorch` templates for `Cloud GPUs` is not possible because they also run your instance in a Docker and you can't run Prover Docker inside your instance Docker.
* Team will provide a `Rust` installation Method in 2 months so we can find cheaper GPU cloud instances easier.


# Installation

## Step 1: Delete Preserved Variables

* Open `/etc/environment`:
```bash
sudo nano /etc/environment
```
Delete everything.

* Add this code to it:
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```

## Step 2: Install & Update Packages
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

## Step 3: Install Docker
```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world
```

## Step 4: Install nVIDIA Cuda
```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
```bash
sudo apt-get update
```
```bash
sudo apt install nvidia-cuda-toolkit
```
```bash
sudo apt-get install -y nvidia-container-toolkit
```
```bash
sudo nvidia-ctk runtime configure --runtime=docker --set-as-default
```
```bash
sudo rmmod nvidia_drm nvidia_modeset nvidia_uvm nvidia
```
```bash
sudo systemctl enable docker
sudo systemctl restart docker

OR

sudo service docker restart
```

Verify installation commands:
* Nvidia driver: `nvidia-smi`
* Cuda toolkits: `nvcc --version`

## Step 5: Set Up HugePages (memory)
The prover node requires `80000 in RAM` in total, and `15000` of it for HugePages (approximately 30 GB of 80 GB memory)

* Check memory: ( you need at least 80 GB)
```bash
free -h
```

* Check current HugePages:
```bash
grep Huge /proc/meminfo
```
Ensure `HugePages_Total` is at least 15000.

* Set HugePages temporarily:
```bash
sudo sysctl -w vm.nr_hugepages=15000
```

* Make HugePages permanently:

Edit `/etc/sysctl.conf` to add:
```bash
sudo nano /etc/sysctl.conf
```
Add the line:
```
vm.nr_hugepages=15000
```
To save: Ctrl + X + Y + Enter

Apply configuration:
```bash
sudo sysctl -p
```

Verify:
```bash
grep Huge /proc/meminfo
```

## Step 6: Build prover
* Clone repo:
```bash
git clone https://github.com/DelphinusLab/prover-node-docker
```
```bash
cd prover-node-docker
```

* Configure `prover_config.json`:
```bash
nano prover_config.json
```
* Set:  
  * `server_url`:`"https://rpc.zkwasmhub.com:8090"`
  * `priv_key`: Your EVM private key (without `0x` prefix).

* Configure `dry_run_config.json`
```bash
nano dry_run_config.json
```
* Set:  
  * `server_url`:`"https://rpc.zkwasmhub.com:8090"`
  * `priv_key`: Your EVM private key same as above (without `0x` prefix).

* Build: 
```bash
bash scripts/build_image.sh
```

## Step 7: Run Prover
* Run:
```bash
bash scripts/start.sh
```

* View Logs:
```bash
cd prover-node-docker

docker compose logs -fn 100
```

* Optional:
  * to Stop Node: `docker compose down -v` or `bash scripts/stop.sh`
  * to Restart Node: `docker compose up -d`
  * Check all docker containers: `docker ps -a`

## Step 8: Check Prover Node Stats
Search your Node address in https://explorer.zkwasmhub.com/

![image](https://github.com/user-attachments/assets/8e6df221-0ca3-4811-a011-61b091e902e5)

