# DelphinusLabs Prover
A step by step guide on running delphinus zkWASM prover and earn credits.


## Requirement
* GPU: RTX 4090
* RAM: >82 GB
* Nvidia CUDA, Docker
* **Local GPU PC** Or **GPU cloud with `VM ubuntu` template**

## Rewards
* If the `liveness-time` is `90%+` and your proving speed keeps stable, then there will be around `$100+ / month estimated reward` + `base` * `number of proofs` you have generated.
* From the current estimate of the overall provers, the **minimal reward** of a **certified** node is around `$200/month`.
the maximal is `$380`.

## Notes
* I have tested it on GPU: `RTX 4090 - 24GB vRAM` and it's so fast with 25tasks/day.
* Team says only supports `4090` but you might need to test with lower GPUs if you want. I believe `24GB vRAM` is 100% needed.
* If you run on GPU cloud providers like `Vast`, you need to install a `VM Ubuntu` template.
* Since the installation is Docker, then `CUDA` or `Pytorch` templates for GPU cloud providers is not possible because they also run your instance in a Docker and you can't run Prover Docker inside your instance Docker
