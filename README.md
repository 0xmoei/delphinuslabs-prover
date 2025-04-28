# DelphinusLabs Prover
A step by step guide on running delphinus zkWASM prover and earn credits.


## Requirement
* GPU: RTX 4090
* RAM: >82 GB
* Nvidia CUDA, Docker
* **Local GPU PC** Or **Cloud GPU with `VM ubuntu` template**

## Rewards
* TGE in Q4. Provers based on their performance share 3% of token allocation at TGE.
* If the `liveness-time` is `90%+` and your proving speed keeps stable, then there will be around `estimated $100+/month` + `base` * `number of proofs` you have generated.
* From the current estimate of the overall provers, the **minimal reward** of a **certified** node is around `$200/month`.
the maximal is `$380`.

## Notes
* I have tested it on GPU: `RTX 4090 - 24GB vRAM` and it's so fast with 25tasks/day.
* Team says only supports `4090` but you might need to test with lower GPUs if you want. I believe `24GB vRAM` is 100% needed.
* The most effiecient way is to use `Local PC GPUs`.
* If you run on `Cloud GPU ` like `Vast`, you need to install a `VM Ubuntu` template.
* Since the Prover installation is using Docker, then `CUDA` or `Pytorch` templates for `Cloud GPUs` is not possible because they also run your instance in a Docker and you can't run Prover Docker inside your instance Docker.
* Team will provide a `Rust` installation Method in 2 months.
