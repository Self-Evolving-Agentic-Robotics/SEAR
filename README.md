<div align="center">

# SEAR: Self-Evolving Agentic Robotics

**A framework and benchmark for robotic agents that improve from their own experience.**

[Xinyi Wang](https://wangxinyilinda.github.io/)<sup>\*,†</sup> ·
[Bangzheng Li](https://www.libangzheng.com/)<sup>\*,†</sup> ·
[Hanyu Wang](https://hywang66.github.io/)<sup>‡</sup> ·
[Lin Zhang](https://lzhangbj.github.io/) ·
[Andrea Yaoyun Cui](https://andreayyc.github.io/) ·
[Zhenggang Tang](https://recordmp3.github.io/) ·
[Shyamal Buch](https://cs.stanford.edu/~shyamal/)<sup>§</sup> ·
[Dejia Xu](https://ir1d.github.io/)<sup>†,§</sup>

<sub><sup>\*</sup> Co-first authors &nbsp;·&nbsp; <sup>†</sup> Project leads &nbsp;·&nbsp; <sup>‡</sup> Real robot lead &nbsp;·&nbsp; <sup>§</sup> Co-last authors</sub>

<br>

[![Website](https://img.shields.io/badge/Website-sear.bot-1f1d1a?style=for-the-badge)](https://sear.bot/)
[![Paper](https://img.shields.io/badge/Paper-coming%20soon-b31b1b?style=for-the-badge)](#release-plan)
[![Code](https://img.shields.io/badge/Code-coming%20soon-lightgrey?style=for-the-badge)](#release-plan)
[![License](https://img.shields.io/badge/License-MIT-2f6f4e?style=for-the-badge)](LICENSE)

</div>

<br>

> [!NOTE]
> **The code is on its way.** Watch or star the repo to get notified when it lands.

<p align="center">
  <img src="assets/teaser.jpg" alt="SEAR self-evolving loop and harness, with real-robot self-evolution sequences, whiteboard writing, and a dexterous hand" width="100%">
</p>

## Overview

Frontier language-model agents can now control a robot and improve by interacting with the environment: they write control code, maintain skills and memories, and even train action models on rollouts they collect themselves. When such an agent's success rises with more interaction, how much of the gain comes from retained experience, rather than from resampling or a longer session? And does that experience generalize to held-out tasks and conditions?

**SEAR** is a framework and benchmark for answering these questions. Its harness connects a language agent with perception and control tools and a persistent library of skills and memories, and runs the same way on simulated and physical robots. The agent can deliver its policy in three forms:

- **Code-as-policy (CaP):** as a program the agent writes and revises.
- **Auto-research (AR):** as a trained checkpoint, by fine-tuning a VLA on its own rollouts.
- **Agent-as-policy (AaP):** as itself, staying in the control loop and choosing actions from live observations.

<div align="center">

| 4+ years | 5 | 330 | 2 | 5 |
| :-: | :-: | :-: | :-: | :-: |
| of agent runtime | frontier models | simulated tasks | physics engines | physical tasks |

</div>

## Why a new benchmark?

A rising success rate does not mean the agent is learning. SEAR pairs each alternative explanation with a matched control:

| The gain might come from… | SEAR's control |
| :-- | :-- |
| Trying more often | Budget-matched best-of-*N* restarts |
| A longer session rather than retained experience | A no-library baseline |
| Practicing on states it will be tested on | A five-tier generalization ladder |
| Reading privileged simulator state | A sealed judge the agent cannot access |
| A lucky run | Replicated lineages from the same configuration |
| Spending more compute | A resource ledger beside wall-clock time |

**The generalization ladder.** T1: new initial states → T2: new conditions → T3: new tasks → T4: new simulator → T5: the real world.

## Testbed

<p align="center">
  <img src="assets/testbed.jpg" alt="Five simulated task families and five physical tasks" width="100%">
</p>

- **Simulation:** 330 tasks in five families on two physics engines, ordered from easiest to hardest: **robosuite**, **LIBERO**, **LIBERO-PRO**, and **LIBERO-RIG** on MuJoCo, and **RoboLab** on Isaac Sim. LIBERO-RIG is new: it keeps each task fixed and perturbs only the robot hardware and its setup.
- **Real world:** two YAM-Ultra arms with five tabletop tasks: Banana into Bowl, Push-T, Cup Upright, Block Stacking, and Spell "SEAR".

## Key findings

1. **The base model sets a floor, and above it the library can reorder models.** A model that rarely writes a working policy has nothing to save: one Inkling-small lineage went 29 tasks in a row without a valid policy. Above that floor, Opus leads every family, but Kimi-k3 overtakes GPT-5.6-sol with a library on three of five. Multimodality is not required: the text-only DeepSeek-v4-flash, given segmentation and depth tools, beats the multimodal Gemini-3.6-flash on RoboLab.
2. **A longer session and an inherited library can each raise performance, but not always.** One long session beats several short ones at the same budget only when a short attempt would end before the model's first success. What the library does depends on what it holds: on RoboLab it raises Opus from 61.9% to 68.0% and Kimi-k3 from 30.1% to 51.9%, but lowers GPT-5.6-sol from 46.9% to 42.4%. Given Opus's library instead, GPT-5.6-sol scores 60.6%.
3. **Experience transfers in part, and the policy form decides which changes it survives.** Control and grasp routines carry across tasks and simulators; task-specific solutions do not. A code policy corrects hardware faults that leave the camera in place but fails when the camera moves, while a VLA trained on its rollouts handles the moved camera. On a real robot, ten practice successes in a row do not predict success on operator-staged scenes.
4. **The loop keeps its mistakes, and agents take shortcuts.** Nothing requires a lesson to be re-checked once it is written down: one agent saved a wrong reading from its own measurement tool as fact, and every later task inherited it, so identically configured lineages diverge. Agents also take shortcuts the benchmark did not intend, such as reading task files that reveal the instruction.

## Release plan

- [x] Project website
- [ ] Paper
- [ ] Code release

## Citation

If you find SEAR useful, please cite:

```bibtex
@article{wang2026sear,
  title   = {{SEAR}: Self-Evolving Agentic Robotics},
  author  = {Wang, Xinyi and Li, Bangzheng and Wang, Hanyu and Zhang, Lin and
             Cui, Andrea Yaoyun and Tang, Zhenggang and Buch, Shyamal and Xu, Dejia},
  year    = {2026}
}
```

## License

Released under the [MIT License](LICENSE).
