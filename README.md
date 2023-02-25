# AIES2021_RL
Reinforcement learning project from the 2021 course on Artificial Intelligence and Expert Systems (AIES)

## Solving The Mountain Car Problem Using Q-Learning and SARSA Methods

### Abstract
In this paper the mountain car problem is solved by using reinforcement learning in both Q-learning and SARSA forms. We demonstrate that both methods can achieve an optimum result and consistently learn to finish a complete episode in under 5 seconds.

### I.	INTRODUCTION
The mountain car problem is a classic reinforcement learning problem introduced by Andrew W. Moore in his PhD thesis titled; “Efficient memory-based learning for robot control” [1]. The problem consists of a car on a 2-dimensional trajectory, trying to climb up a steep hill on the right. But the maximum acceleration of the car is not enough to overcome gravity and friction by itself. Therefore, in order to complete its task, the car needs to drive forward and backwards to and build up enough momentum to go up the hill. The agent receives a reward of +1 only if it reaches the goal, and also receives a reward of -0.01 at each step, in order to finish each episode as soon as possible. A naïve representation of the problem is depicted on Figure 1.

![image](https://user-images.githubusercontent.com/65850584/221375367-a869ae87-3cbb-44a1-96f6-7043d2e04a64.png)
Figure 1. Schematic representation of the mountain car problem

The position is determined by x and y=sin(3x). The x argument of the position is in the range of [-1.2, 0.5] meters and the velocity is limited to the range of [-1.5, 1.5] meters per second. The state space equations are given as follows:

![image](https://user-images.githubusercontent.com/65850584/221375406-ed696182-b6e4-4211-b0d4-95b1672c3f2f.png)

In which x and ẋ represent the position and velocity of the car. Δt represents the time step which equals 0.1 s. g=9.81m/s2 is the earth’s gravitational constant. m=0.2 kg is the mass of the car. at is the action taken at time t, which can be one of three values [-2, 0, 2] measured in Newtons. And finally, k= 0.3 N is the frictional constant.

The experiments conducted in this paper are divided into three steps, discussed in sections 2 to 4. In the second section Q-learning and SARSA methods are utilized to solve the problem. In the third section the effect of noise implementation on the environment is studied. The fourth section analyzes the effect of different learning parameters on the outcome. Finally, the fifth section concludes the results of the paper.

### II.	IMPLEMENTATION OF Q-LEARNING AND SARSA METHODS
In order to simplify the problem, the Q-values of all state-action pairs are represented in a 3-dimensional matrix. For this purpose, the state space needs to be discretized. Based on trial and error, the position and velocity values of state are discretized into 18 and 7 values respectively. Since the only positive goal is reached at the end of each episode, a discount factor of 0.999 is chosen to give more importance to future rewards. The learning rate is chosen to be 0.1. And the initial value of epsilon for the epsilon greedy method is chosen to be 0.2 which decays over time during the training process. For this purpose, the exponential decay formula is chosen which is as follows:

![image](https://user-images.githubusercontent.com/65850584/221375470-ac188a79-8614-4171-b933-680de5b47b37.png)

In which εk is the value of epsilon on the kth epoch. k ranges from 0 to n, which is the final epoch number. And d represents the exponential decay rate. The changing pattern of epsilon throughout the training process for different values of the decay rate and with an initial epsilon of 0.5, is shown on Figure 2. In order to balance exploration and exploitation in the solution, a decay rate of 10 is chosen which yields in acceptable results.

![image](https://user-images.githubusercontent.com/65850584/221375481-a610b0cb-f71d-438f-b4a7-b6703fe88405.png)
Figure 2. Decaying behavior of epsilon throughout the training process

The number of steps taken in each epoch is 50, which forces the algorithm to learn to finish each episode and reach the reward in under 5 seconds. The total number of epochs is 20k, in which the agent consistently learns to accomplish its task. For the first two thirds of the epochs, the agent is exploring, which means the initial state and action are randomly chosen, but in the last one third, the initial state is always [0,0] and the initial action is chosen based on the policy, in order to learn to accomplish the task in its actual form. In the following, the results of using Q-learning and SARSA methods are discussed. In most of the trials, both methods have proved to be able to complete the task in under 5 seconds, and one of the best results of each method has been represented here in more detail.

#### A.	Q-Learning
The training process takes 19.57 seconds and the agent learns to complete the task in 3.6 seconds which is remarkably good. The accumulative return of each epoch during the training process creates a graph as represented in Figure 3. The animation of the car climbing up the hill, the policy matrix, the Q-values of state-action pairs, and the array of accumulative return values trained by Q-learning are available in the Q-Learning folder.

![image](https://user-images.githubusercontent.com/65850584/221375515-0958c2b6-3208-4107-abd8-c8d5dce89761.png)
Figure 3. Accumulative return during the training by Q-learning

#### B.	SARSA
The training process takes 16.3 seconds and the agent learns to complete the task in 3.7 seconds which is also very good. The accumulative return of each epoch during the training process creates a graph as represented in Figure 4. The animation of the car climbing up the hill, the policy matrix, the Q-values of state-action pairs, and the array of accumulative return values trained by SARSA are available in the SARSA folder.

![image](https://user-images.githubusercontent.com/65850584/221375525-4848673b-7932-4188-9ebd-084a318a210a.png)
Figure 4. Accumulative return during the training by SARSA

In both Q-learning and SARSA methods, the accumulative return has a sudden decrease around the 13500th epoch. The reason for this jump is because for the last one third of the epochs, the algorithm is supposed to learn the whole episode from the initial states.

Over many rounds of trial and error, the SARSA method has proven to give more consistent results, also being faster and showing a smoother trend in training. For this reason, the SARSA method is used for the rest of the experiments of the paper.

### III.	EFFECT OF NOISE
The noise is implemented by choosing a random action in each step with the probability of the noise percentage. This method of choosing a random action differs from the epsilon greedy method or the initial random action of each epoch, because the algorithm knows the action to be its desired, and dedicates the Q-value to that action, while in fact it might be incorrect and the actual action be randomly chosen. Here two noise probabilities of %20 and %30 are tested. For the noisy environment, the number of steps in each epoch is increased to 100 since it is much more difficult for the agent to learn to finish the task in under 5 seconds in a noisy environment.

#### A.	%20 Noise
The total training time with a %20 noisy environment is 29.20 seconds, which is an acceptable result, considering the increase of the number of steps in each epoch. The agent learns to complete the task in 6.0 seconds. The accumulative return of each epoch during the training process creates a graph as represented in Figure 5. It is apparent from this graph, that the training process is not as smooth as the 0 noise environment training shown on Figure 4.

![image](https://user-images.githubusercontent.com/65850584/221375562-721d3544-c329-4740-8db0-d84652e4e620.png)
Figure 5. Accumulative return during the training by SARSA with %20 noise

#### B.	%30 Noise
The total training time with a %30 noisy environment is 36.57 seconds, which is much higher than the %20 noisy environment. This shows the effect of increasing the noise on the training process and the struggle of the algorithm in training in a noisy environment. But remarkably the agent learns to complete the task in 5.8 seconds, even faster than the %20 noisy environment. This result is completely by chance and cannot be taken for granted. If the training process is repeated for many times, it can be proven that a less noisy environment yields better results, since it’s less hindered by noise. The accumulative return of each epoch during the training process creates a graph as represented in Figure 6. As shown in the graph, the training process shows even more oscillations compared to the training of the %20 noisy environment in Figure 5.

![image](https://user-images.githubusercontent.com/65850584/221375602-bf7cca55-dab1-4def-a6f0-736cd142fe70.png)
Figure 6. Accumulative return during the training by SARSA with %30 noise

### IV.	EFFECT OF DIFFERENT LEARNING PARAMETERS
For the previous experiments, different learning parameters were optimally chosen based on trial and error. However, in this section we are going to study the effect of changing each of these parameters on the learning process of the algorithm. For this purpose, four different values for each of the discount factor, learning rate, and initial epsilon are tested. Once again, because the values tested in this experiment are not optimal, the number of steps in each epoch is decreased to 100 in order to give the agent more chance of accomplishing its task.

#### A.	Discount Factor
The discount factor plays a crucial role in determining the importance of the distant future rewards compared to the near future ones. This might lead one to think that since the reward is at the end of the episode in our problem, a higher discount factor is needed to accomplish the task. This however, is not entirely true. While it’s true that that the only important reward is the furthest one in the future, snice there are no other positive rewards to compare it to, and the near future negative rewards are too small, the agent can learn the task with any given discount factor, unless it’s so small, that the -0.01 step reward becomes important and the agent tries to avoid the negative rewards instead of trying to reach the positive reward.

For this experiment discount factors of 0.7, 0.4, 0.1 and 0.01 are tested. The results of which are shown on Figure 7 by plotting the accumulative return during training. The ultimate trained agent with each of the first three discount factors can complete the task in 4.7, 7.9 and 5.2 seconds. These differences are not important. The only important thing to note is that the agent can learn the task with each of the 0.7, 0.4 and 0.1 discount factors, but by using 0.01 as the discount factor, the agent struggles to learn and eventually cannot accomplish the task.

#### B.	Learning Rate
The learning rate determines the rate at which the algorithm learns the Q-value of each state-action pair at each step. A higher value leads to faster and noisier training process, while a lower value leads to a smoother but slower training. If the discretization of states were to be finer, with more discrete states, a higher learning rate would have yielded in better results. On the other hand, if a noisy environment were present, using lower learning rate values would have been advised.

![image](https://user-images.githubusercontent.com/65850584/221375657-860e7afa-b7fc-43d8-a95a-42d1b595d5b4.png)
Figure 7. Accumulative return during training with different disctount factors

For this experiment, values of 0.5, 0.01, 0.001 and 0.0001 are chosen as learning rates. The results of which are shown on Figure 8 by plotting the accumulative return during training. The ultimate trained agent with each of the first three learning rates can complete the task in 7.1, 6.0 and 9.9 seconds. With the 0.0001 learning rate however, the agent fails to learn the task.

As it is evident from the resulting completion times and the accumulative return graphs, increasing the learning rate to 0.5 results in a much noisier training process and doesn’t yield a good result. The reason for this is that the state space is not finely discretized. Therefore, each discrete state can represent different continuous states, requiring different actions. Learning the appropriate action for each of these states with a high learning rate, results in such a noisy learning trend.

While decreasing the learning rate to 0.01 has yielded a smoother training process with acceptable results, it counteracts the exploration and may not find the optimal result. Decreasing the learning rate even further, results in more struggle in training. With learning rate = 0.001 the agent can only just finish its task in time, and with lower learning rates it fails to learn the task.

#### C.	Initial Epsilon
The value of epsilon determines the amount of exploration during training. The decay rate is just as much important as the initial value of the epsilon, but for this experiment the optimal decay rate of 10 is kept constant and initial epsilon values of 1, 0.1, 0.01 and 0.001 are tested. The results of which are shown on Figure 9 by plotting the accumulative return during training. The ultimate trained agent with each of the initial epsilons can complete the task in 4.2, 4.0, 3.7 and 4.2 seconds. While the specific difference between these values is not important, it is important to note that the agent can learn the task even with the smallest epsilon values, which means that in this task, exploitation is more important than exploration, since the only positive reward is at the end of the episode and once it’s known, it’s only a matter of time for the agent to learn how to get there. Yet it’s still important to have some amount of exploration in order to achieve the optimal result.

### V.	CONCLUSION
In this paper the mountain car problem has been solved by utilizing Q-learning and SARSA methods. We have demonstrated that even by grossly discretizing the state space for this problem, the agent can consistently learn to perform its task in under 5 seconds which is a remarkable result by itself, but can sometimes reach as low as under 4 seconds.

We have demonstrated that the optimal learning parameters for the task at hand are gamma=0.999, alpha=0.1, initial epsilon=0.5 and the exponential decay rate for epsilon=10. Having proven these by trial and error, it’s been also proved that the amounts of gamma and initial epsilon do not play a great role in the outcome, unless gamma is too small, or initial epsilon is too great or too small.

The effect of noise has also been studied. And it has been proven that the agent can learn the task even in a %20 and %30 noisy environment.

![image](https://user-images.githubusercontent.com/65850584/221375694-ddc19e98-bcbe-46b9-aef7-84fef396f2a6.png)
Figure 8. Accumulative return during training with different learning rates

![image](https://user-images.githubusercontent.com/65850584/221375703-e78b0a01-4253-435e-b1a7-5e50bac6aea5.png)
Figure 9. Accumulative return during training with different initial epsilon values

### REFERENCES
[1]	Andrew William Moore, “Efficient memory-based learning for robot control,” University of Cambridge, 1990.


