[//]: # (Image References)

[image2]: https://github.com/olivierharel/continunous-control-rdl/blob/81c69e44294b69e5838b0d5f8b5eba5bd8528541/Training-score-curve.png "Training score curve"
[image3]: https://github.com/olivierharel/continunous-control-rdl/blob/4d6c50240565a58746094f24410b806925f8cffa/trained10.webp "Trained agent"
[image4]: https://github.com/olivierharel/continunous-control-rdl/blob/4d6c50240565a58746094f24410b806925f8cffa/untrained10.webp "Untrained agent"

# Continuous Control project (Reacher) report

### Hyper parameters

The code uses the following hyper parameters.

1. ddpg() parameters:
**max_t** sets the maximum number of timesteps per episodes.
**update_period** and **training_per_update** set the relative frequency of training phases and data gathering phases:
- At each timestep, in an episode 20 experiences are gathered and added to the replay buffer
- update_period specifies the period, in timesteps, between training phases of the actor-critic architecture
- training_per_update specifies the number fo training iterations within a training phase

2. Training parameters in are listed at the top of ddpg_agent_ac.py:
- **BUFFER_SIZE** = int(1e5)  # replay buffer size
- **BATCH_SIZE** = 128        # minibatch size
- **GAMMA** = 0.99            # discount factor
- **TAU** = 1e-3              # for soft update of target parameters (DDPG/DQN)
- **LR_ACTOR** = 5e-4         # learning rate of the actor 
- **LR_CRITIC** = 5e-4        # learning rate of the critic
- **WEIGHT_DECAY** = 1e-5     # L2 weight decay
- **ACT_FC1** = 200           # Hidden size of the actor network (layer 1)
- **ACT_FC2** = 100           # Hidden size of the actor network (layer 2)
- **CRI_FC1** = 200           # Hidden size of the critic network (layer 1)
- **CRI_FC2** = 128           # Hidden size of the critic network (layer 2)
- **CRI_FC3** = 128           # Hidden size of the critic network (layer 3)
- **EPSILON_DECAY** = 0.9996  # Decay (per timestep) of the noise scaling factor (epsilon)

### Training observations

Some key parameters did impact the success and stability of training more than others.
1. Setting the maximum number of iterations within an episode to a large number: 1000 or more
If restricted to < 300, learning does not happen. It took me a while to realize this was a problem.
My intution is that the agent takes a good action, this may lead to more experiences around a high-reward state.

2. The state/action spaces dimensions are small. Yet I found that if the hidden layer sizes are < 128, training is tends to be unstable

3. The size of the replay buffer provides a trade-off between training stability (larger) and convergence speed (smaller)
Given 1000-2000 steps per episodes, 20 expeirences per step, a replay buffer of size 10^5 covers about 3-4 episodes.

4. Starting with a value of training_per_update (10) yields fast and stable training. That number can subsequently be reduced.

With the committed settings, training was fast and stable:
![Training score curve][image2]

Side-by-side comparison of untrained and trained agents:
| Untrained | Trained |
|:---------:|:-------:|
| ![Untrained][image4] | ![Trained][image3] |

### Future work
- Apply to more complex problems (eg Crawler with walking jointed agents)
- Implement an asynchronous architecture
- Try different actor-critic architectures (eg A3C)






