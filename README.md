# DEP-RL: Embodied Exploration for Reinforcement Learning in Overactuated and Musculoskeletal Systems

 This repo contains the code for the paper [DEP-RL: Embodied Exploration for Reinforcement Learning in Overactuated and Musculoskeletal Systems](https://openreview.net/forum?id=C-xa_D3oTj6) paper, published at ICLR 2023 with perfect review scores (8, 8, 8, 10) and a notable-top-25% rating. See [here](https://sites.google.com/view/dep-rl) for videos.

The work was performed by Pierre Schumacher, Daniel F.B. Haeufle, Dieter Büchler, Syn Schmitt and Georg Martius.

If you just want to see the code for DEP, take a look at `deprl/dep_controller.py`, `deprl/custom_agents.py` and `deprl/env_wrapper/wrappers.py`


## Abstract
Muscle-actuated organisms are capable of learning an unparalleled diversity of
dexterous movements despite their vast amount of muscles. Reinforcement learning (RL) on large musculoskeletal models, however, has not been able to show
similar performance. We conjecture that ineffective exploration in large overactuated action spaces is a key problem. This is supported by our finding that common
exploration noise strategies are inadequate in synthetic examples of overactuated
systems. We identify differential extrinsic plasticity (DEP), a method from the
domain of self-organization, as being able to induce state-space covering exploration within seconds of interaction. By integrating DEP into RL, we achieve fast
learning of reaching and locomotion in musculoskeletal systems, outperforming
current approaches in all considered tasks in sample efficiency and robustness.

## Installation

We recommend an installation with [poetry](https://python-poetry.org/) to ensure reproducibility.
While [TonicRL](https://github.com/fabiopardo/tonic) with PyTorch is used for the RL algorithms, DEP itself is implemented in JAX. We *strongly* recommend GPU-usage to speed up the computation of DEP. On systems without GPUs, give the tensorflow version of TonicRL a try! We alternatively provide a pip_requirements file.
1. Make sure to install poetry and deactivate all virtual environments.
2. Clone the environment
```
git clone updatelink!
```

3. Go to the root folder and run
```
poetry install
poetry shell
```

That's it!


You can try the installation by running:
```
bash play_files/play_dep_humanreacher.sh
bash play_files/play_dep_ostrich.sh
bash play_files/play_dep_dmcontrol_quadruped.sh
```

You might see a more interesting ostrich behavior by disabling episode resets in the ostrich environment first.

The build has been tested with:
```
Ubuntu 20.04 and Ubuntu 22.04
CUDA 12.0
poetry 1.4.0
```
### Troubleshooting
* A common error with poetry is a faulty interaction with the python keyring, resulting in a `Failed to unlock the collection!`-error. It could also happen that the dependency solving takes very long (more than 60s), this is caused by the same error.
If it happens, try to append
```
export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring
```
to your bashrc.
* If you have an error related to your `ptxas` version, this means that your cuda environment is not setup correctly and you should install the cuda-toolkit. The easiest way is to do this via conda if you don't have admin rights on your workstation.
I recommend running
```
conda install -c conda-forge cudatoolkit-dev
```


Feel free to open an issue if you encounter any problems.

## Experiments

The major experiments (humanreacher reaching and ostrich running) can be repeated with the config files.
Simply run from the root folder:
```
python -m deprl.main experiments/ostrich_running_dep.json
python -m deprl.main experiments/humanreacher.json
```
to train an agent. Model checkpoints will be saved in the `output` directory.
The progress can be monitored with:
```
python -m tonic.plot --path output/folder/
```

To execute a trained policy, use:

```
python -m deprl.play --path output/folder/
```

See the [TonicRL](https://github.com/fabiopardo/tonic) documentation for details.

Be aware that ostrich training can be seed-dependant, as seen in the plots of the original publication.

## Environments

The ostrich environment can be found [here](https://github.com/vittorione94/ostrichrl) and is installed automatically via poetry.

The underlying arm-environment (warmup) can be used like any other gym environment:

```
import gym
import warmup

env = gym.make("humanreacher-v0")

for ep in range(5):
     ep_steps = 0
     state = env.reset()
     while True:
         next_state, reward, done, info = env.step(env.action_space.sample())
         env.render()
         if done or (ep_steps >= env.max_episode_steps):
             break
         ep_steps += 1

```

## Citation

Please use the following citation if you make use of our work:

```
@inproceedings{schumacher2023:deprl,
  title = {DEP-RL: Embodied Exploration for Reinforcement Learning in Overactuated and Musculoskeletal Systems},
  author = {Schumacher, Pierre and Haeufle, Daniel F.B. and B{\"u}chler, Dieter and Schmitt, Syn and Martius, Georg},
  booktitle = {Proceedings of the Eleventh International Conference on Learning Representations (ICLR)},
  month = may,
  year = {2023},
  doi = {},
  url = {https://openreview.net/forum?id=C-xa_D3oTj6},
  month_numeric = {5}
}
```
