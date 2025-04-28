# DelphinusLabs Prover
A step by step guide on running delphinus zkWASM prover and earn credits.


## Requirement
* GPU: min. RTX 4090
* RAM: >82 GB
* Nvidia CUDA, Docker
* **Local GPU PC** Or **Cloud GPU with `VM ubuntu` template**

## Rewards
* TGE in Q4. Provers based on their performance share 3% of token allocation at TGE.
* If the `liveness-time` is `90%+` and your proving speed keeps stable, then there will be around `estimated $100+/month` + `base` * `number of proofs` you have generated.
* From the current estimate of the overall provers, the **minimal reward** of a **certified** node is around `$200/month`.
the maximal is `$380`.

## Notes
* I have tested it on GPU: `RTX 4090 - 24GB vRAM` **($0.41/hr in [Vast.io](https://cloud.vast.ai/?ref_id=228875))** and it's so fast with 25tasks/day. In my case, it might even NOT be profitable at TGE, I just love experiment.
* Team says only supports `4090` but you might need to test with lower GPUs if you want. I believe `24GB vRAM` is 100% needed.
* The most effiecient way is to use `Local PC GPUs`.
* If you run on `Cloud GPU ` like [Vast.io](https://cloud.vast.ai/?ref_id=228875), you need to install a `VM Ubuntu` template. and NOT `CUDA` or `Pytorch`.
* Since the Prover installation is using Docker, then `CUDA` or `Pytorch` templates for `Cloud GPUs` is not possible because they also run your instance in a Docker and you can't run Prover Docker inside your instance Docker.
* Team will provide a `Rust` installation Method in 2 months so we can find cheaper GPU cloud instances easier.


# Installation

### Install nVIDIA Cuda
```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y nvidia-container-toolkit
```
```
sudo nvidia-ctk runtime configure --runtime=docker --set-as-default
```
```
sudo systemctl enable docker
sudo systemctl restart docker

OR

sudo service docker restart
```

Verify installation commands:
* Nvidia driver: `nvidia-smi`
* Cuda toolkits: `nvcc --version`

### Set Up HugePages
The prover node requires `80000 in RAM` in total, and `15000` of it for HugePages (approximately 30 GB of 80 GB memory)

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


