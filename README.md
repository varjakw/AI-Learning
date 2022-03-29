# AI-Learning
Q-Learning, Markov Decision Processes and an assignment in Python
## Markov Decision Process
The MDP is when an agent uses reinforcement learning to decide the best action to take in the current context. An MDP contains:

- A set of possible world states S.
- A set of Models.
- A set of possible actions A.
- A real-valued reward function R(s,a).
- A policy for the solution of Markov Decision Process.

A state is is a set of tokens that represent every state the agent can be in.

A model, often called a transition model, gives an action's effect in a state. ```T(S, a, S’)``` defines a transition ```T``` where being in state ```S``` and taking an action ```a``` will take us to state ```S'``` (```S``` and ```S'``` may be the same.) For on-deterministic actions, we also define a probability ```P(S'|S,a)``` which represents the probability of reaching a state ```S'``` if action ```a``` is taken in state ```S```.

An action is a set of all possible actions. ```A(s)``` defines the set of actions that can be taken being in state ```S```.

A reward is a real-valued reward function. ```R(s)``` indiciates the reward for simply being in the state ```S```. ```R(S,a)``` indicates the reward for being in a state ```S``` and taking an action ```a```. ```R(S,aS')``` is the reward for being in state ```S```, taking action ```a``` and ending up in state ```S'```.

A policy is the solution to the MDP. It is a mapping from ```S``` to ```a```. It inidicates the action to be taken while in the state.

### Example: Grid World
![image](https://user-images.githubusercontent.com/78870995/160612358-45dfb138-303b-4449-96ce-6b6811a78afe.png)

An agent lives in a 3x4 grid. The grid has a ```START``` state at grid (1,1). The purpose of the agent is to wander around the grid and finally reach the blue diamond at grid (4,3). The agent should avoid the Fire grid at (4,2). Also, grid (2,2) is blocked so the agent cannot enter it. The agent can move up, down, left or right.

If there is a wall in the direction the agent would have taken, the agent remains in the same place.

Aim: Find the shortest sequence from starting state to the diamond. Two such sequences can be found:

- RIGHT RIGHT UP UP RIGHT
- UP UP RIGHT RIGHT RIGHT

We will examine the second one. Lets say the move is now noisy, 80% of the time, the intendeed action occurs. 20% of the time, the actuon causes it to move at right angles. e.g. if the agent says UP, the probability of going UP is 0.8, while the probability of going LEFT or RIGHT is 0.1 each, as they are both right angles to UP.

The agent recieves rewards each time it takes a step:
- Small reward each step (can be negative as punishment, like entering the fire)
- Big rewards come at the end (good or bad)
- The goal is to maximise the sum of rewards.

This leads us on to:

## Reinforcement Learning
RL is about taking suitable action to maximize reward. It is used to find the best possible behaviour or path to be taken in a specific context. This is different from supervised learning which has the answer key with it so the model is trained with the correct answer in mind, while reinforcement learning is not provided the answer and needs to find it itself.

- Input: An initial state from which the model will start
- Output: There are many possible outputs as there can be a variety of solutions to a particular problem.
- Training: The training is based upon the input, the model will return a state and the user will decided to reward or punish the model based on its output.
- - The model keeps learning
- - The best solution is decided based on the maximum reward.

### MDP Example: Restaurant

- Action: a menu item ordered
- State: a subset ```s``` of menu items, specifying whats available.
- In general, an order does not affect whats available ![image](https://user-images.githubusercontent.com/78870995/160616418-e6c954bc-2926-42c1-a881-f6fb0dfbd975.png)
  - (1) Sometimes though, ordering an available item enough times may make it unavailable like if supply runs out.
  - (2) Ordering an unavailable item enough times may push the restaurant to make it available (like bringing back a special by popular demand).
- Treat "enough times" in (1) and (2) with the same degree ```δ```
![image](https://user-images.githubusercontent.com/78870995/160617090-aa90ff9b-4273-451a-909b-44c87302f510.png)
where ```s_a``` is ```s``` changed according to (1) and (2).

#### Rewards as a basis of policy
Turn a rating ![image](https://user-images.githubusercontent.com/78870995/160617361-af7aac2c-d381-4c2c-a2a9-5266248cd418.png)
of how delicious  ```a``` is into:
![image](https://user-images.githubusercontent.com/78870995/160617442-90603919-4d95-41e8-b1eb-abe558631cae.png)
