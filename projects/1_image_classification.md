---
layout: page
title:  1 - Image Classification
description: >
  Unit 1 notes. Jupyter notebook for this page can be found [**here**](https://github.com/ASBattu/MachineLearning_Tensorflow/blob/main/1_Image_Classification.ipynb)
hide_description: false
sitemap: false
---

1. this list will be replaced by the toc
{:toc .large-only}

## Introduction aaa
In this experiment, a lander agent is trained to safely land on the surface between the two flags.

## Trained model vs untrained model
- First, a difference between a trained and an untrained agent.
- An untrained agent takes random actions in the environment and fails whereas the trained agent calculates its relative position in the environment and using the jets, lands safely.
- This page describes how the agent was trained.

<img src="https://media.giphy.com/media/7z03MkZdDyPmAPZ1D8/giphy.gif" width="400"/> <img src="https://media.giphy.com/media/OxACWobLRgTpog3XZ8/giphy.gif" width="400"/>

Trained vs Untrained agent
{:.figure}

## Libraries and environment used
- [**Gym**](https://github.com/openai/gym) is an open source Python library for developing and comparing a standard API to communicate between learning algorithms and environments. It is a diverse collection of reference environments
- [**Stable Baselines3 (SB3)**](https://stable-baselines3.readthedocs.io/en/master/) is a set of reliable implementations of reinforcement learning algorithms in PyTorch. Algorithms used in this experiment come from SB3 and various others can be chosen based on the environment.

- [**LunarLander-v2**](https://www.gymlibrary.ml/environments/box2d/lunar_lander/) environment is used for this project. The objective of LunarLander is to **safely land a spaceship between the two flags**. The LunarLander environment is:
  - Fully Observable - All necessary state information is known at every frame
  - Single Agent - No competition
  - Deterministic - No stochasticity
  - Episodic - Reward depends only on the current state and action

## Defined functions
- **init_render()** - Initially create the environment and render it for 150 timesteps.
- **play_trained_model()** - Once a model is trained, this function plays the model for one episode to view progress.
- **record_random_episode()** - To record a 'gif' of the agent playing in the environment with **random inputs** for documentation purposes.
- **record_trained_model_episode()** - To record a 'gif' of the agent playing in the environment with a **trained model** for documentation purposes.
- **evaluate_model()** - Evaluation helper from SB3, runs the trained policy for 'n' number of episodes and returns the average reward for the agent in the environment.

## Environment Action space and Observation space
- LunarLander-v2 Action Space:  Discrete(4)
  - 4 Discrete actions can be taken - thrust, left, right, nothing
- LunarLander-v2 Observation Space  Box([-inf -inf -inf -inf -inf -inf -inf -inf], [inf inf inf inf inf inf inf inf], (8,), float32)
  - 8 observations - **Continuous** (X distance from target site, Y distance from target site, X velocity, Y velocity, Angle of ship, Angular velocity of ship) and **Binary** (Left leg is grounded, Right leg is grounded).


## SB3 models to choose from
As explained above, SB3 is a set of implementations of reinforcement learning algorithms. As the environment has **Discrete** action space and **Box** observation space, I will be testing these three algorithms: PPO, A2C and DQN (as described in the project discription page). **MlpPolicy** is a policy that implements actor critic, using a MLP(2 layers of 64)


## PPO Implementation (MLP Policy)
In simple terms, the Proximal Policy Optimization algorithm combines ideas from A2C (having multiple workers) and TRPO (it uses a trust region to improve the actor). The main idea is that after an update, the new policy should be not too far from the old policy. For that, ppo uses clipping to avoid too large update. Minus this small description, more information can be found [here](https://stable-baselines3.readthedocs.io/en/master/modules/ppo.html).

## A2C Implementation
Again, in simpler terms, A2C is a synchronous, deterministic variant of Asynchronous Advantage Actor Critic (A3C). It uses multiple workers to avoid the use of a replay buffer. It uses a asynchronous gradient descent for optimization of deep neural network controllers. More information can be found [here](https://stable-baselines3.readthedocs.io/en/master/modules/a2c.html)

## DQN Implementation
Deep Q Network builds on 'Fitted Q-Iteration' and make use of different tricks to stabilize the learning with neural networks: it uses a replay buffer, a target network and gradient clipping. More information can be found [here](https://stable-baselines3.readthedocs.io/en/master/modules/dqn.html)

## Conclusion and Results
- All agents were trained only for 500k timesteps - Due to computational reasons and how this page is just to showcase basic experience in setting up a GYM environment and testing out basic RL SB3 policies.
- Training each policy for more timesteps usually increases the average reward per episode, upto a limit though.
- for 500k timesteps, PPO performs the best with an average mean reward of '231.71 +/- 19.68', then DQN and finally A2C.

- These results heavily depend on the number of timesteps trained and hyperparameters optimized, therefore I'll end this with, more experimentation is needed to get a conclusive result.

## Publishing best model on huggingface

