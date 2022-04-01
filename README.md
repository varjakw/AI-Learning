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

In an MDP consisting of ```<S,A,T,R>```:
- The state space ```S``` is a set of all the states that the agent can transition to.
- The action space ```A``` is a set of all the actions the agent can do in a certain environment

In a Grid World context, the agent starts from the ```START``` state and uses reinforcement learning to reach a rewarding grid. The episode ends when the agent reaches the goal and obtains +1 reward or reaches the negative punishment -1 such as going through the fire. At every time setep, the agent is allowed to choose from a set of actions (the action space ```A = [Right, Left, Up, Down]```). The decision on which action to take is either decided by a calculated value (Value Iteration) or an iteratively evaluated and updated state-action mapping (Policy)

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

Add look ahead ```n``` to ```Q_n```: for ![image](https://user-images.githubusercontent.com/78870995/160618551-55197733-e40f-48b0-8b6c-a7e82da43f69.png)
and ![image](https://user-images.githubusercontent.com/78870995/160618611-2ac32ede-3f72-44a5-a321-e7badcd688cc.png)

But what if we dont know ```r0``` or ```δ``` (but know the discount γ)

Q-Learning corrects some initial arbitrary valuation ![image](https://user-images.githubusercontent.com/78870995/160619042-db4f9c59-fb1a-412b-8694-622bb393e16b.png)
using an initial state ```s1``` and an ε-greedy policy to choose an action ```a_n``` for the update.
![image](https://user-images.githubusercontent.com/78870995/160619319-c154facf-c2c7-4045-b942-809c722d466f.png)

## Q-Learning
QL is a basic form of RL which uses Q-values (also called action values) to iteratively improve the behaviour of the learning agent.

- Q-values are defined for state and actions. ```Q(S,A)``` is an estimation of how good is it to take the action ```A``` at the state ```S```. This estimation of ```Q(S,A)``` will be iteratively computed using the TD-Update rule which we will see.
- Rewards and Episodes: An agent over the course of its lifetime starts from a start state, makes a number of transitions from its current state to a next state based on its choice of action and also the environment the agent is interacting in. At every step of the transition, the agent from a state takes an action, observes a reward from the environment and then transits to another state. If at any point of time the agent ends up in one of the terminating states, that means there are no further transitions possible. This is said to be the completion of an episode.
- Temporal Difference or TD-Update can be written as:
![image](https://user-images.githubusercontent.com/78870995/160621663-8ab41421-ae6e-446f-9b2b-50fa323a0567.png)
This update rule to estiamte the value of Q is applied at every step of the agents interaction with the environment.
  - ```S```: current state of the agent
  - ```A```: current action picked according to a policy
  - ```S'```: next state where the agent ends up
  - ```A'```: next best action to be picked using current Q-value estimation i.e. pick the action with the maximum Q-value in the next state.
  - ```R```: current reward observed from the environment in response of current action. 
  - ```γ```(>0 and <=1): discounting factor for future rewards. Future rewards are less valuable than current rewards so they must be discounted. Since Q-value is an estimation of expected rewards from a state, the discounting rule applies here as well.
  - ```α```: step length taken to update the estimation of ```Q(S,A)```.
- Choosing the action to take using ε-greedy policy. This is a very simple policy of choosing actions using current Q-value estimations which goes as follows:
  - With probability ```(1-ε)``` choose the action which hass the highest Q-value. (5.1 in assignment)
  - With probability ```(ε)```   choose any action at random. (5.2 in assignment)
