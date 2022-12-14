[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/43851024-320ba930-9aff-11e8-8493-ee547c6af349.gif "Trained Agent"
[image2]: https://github.com/olivierharel/continunous-control-rdl/blob/81c69e44294b69e5838b0d5f8b5eba5bd8528541/Training-score-curve.png "Training score curve"
[image3]: https://github.com/olivierharel/continunous-control-rdl/blob/4d6c50240565a58746094f24410b806925f8cffa/trained10.webp "Trained agent"
[image4]: https://github.com/olivierharel/continunous-control-rdl/blob/4d6c50240565a58746094f24410b806925f8cffa/untrained10.webp "Untrained agent"

# Continuous Control project (Reacher)

### Introduction

This project uses the [Reacher](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#reacher) environment from Unity (adapted by Udacity).

![Trained Agent][image1]

In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

### Distributed DDPG Training

The environment contains 20 identical agents, each with its own copy of the environment. Distributed training is used to gather experience across multiple copies ('workers') of the agents.
It is commonly used in algorithms like [PPO](https://arxiv.org/pdf/1707.06347.pdf), [A3C](https://arxiv.org/pdf/1602.01783.pdf), and [D4PG](https://openreview.net/pdf?id=SyZipzbCb).

Here we use a distributed version of [DDPG (Deep Deterministic Policy Gradient)](https://arxiv.org/pdf/1509.02971.pdf), an actor-critic architecture, to learn an optical policy.

Training is considered complete, when the average, over the last 100 episodes, of the average scores across worker-agents is >= +30. 

###  Setup the project in your environment

0. Dependencies

1. Download the project from Github ('reacher/')

2. Download the environment from one of the links below.  You need only select the environment that matches your operating system:
    - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Linux.zip)
    - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher.app.zip)
    - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Windows_x86.zip)
    - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Windows_x86_64.zip)
    
    (_For Windows users_) Check out [this link](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64) if you need help with determining if your computer is running a 32-bit version or 64-bit version of the Windows operating system.

    (_For AWS_) If you'd like to train the agent on AWS (and have not [enabled a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md)), then please use [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Linux_NoVis.zip) (version 1) or [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Linux_NoVis.zip) (version 2) to obtain the "headless" version of the environment.  You will **not** be able to watch the agent without enabling a virtual screen, but you will be able to train the agent.  (_To watch the agent, you should follow the instructions to [enable a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md), and then download the environment for the **Linux** operating system above._)

3. Place the file in the reacher/ folder, and unzip it. 

### Instructions

a. There are 3 main program files:
- `Continuous_Control.ipynb`: the notebook sets up the environment and orchestrates the training
- `ddpg_agent_ac.py`: defines multiple classes (agent: the worker class, actor_critic; the learning architecture class)
                    as well as fundamental hyperparemters (network dimensions, training hyperparameters...)
- `model.py`: the DL networks used for the critics and actors 

b. `Continuous_Control.ipynb` has 4 sections:
  1. `Setup the general environment`: load the unity environment
  2. `Observe the environment space`: printout fundamental information about the environment (size of state/action spaces...)
  3. `Take random actions in the environment`: observe untrained agent
  4. `It's your turn`: learn an effective policy with Distributed-DDPG
  5. `Replay of trained agent`

b. To learn, execute steps 1, 2 and 4.
c. To replay a trained agent, execute steps 1, 2 and 5.

d. The weights are saved as:
- actor: `checkpoint_actor.pth`
- critic: `checkpoint_critic.pth`

