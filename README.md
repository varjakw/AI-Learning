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

A model, often called a transition model, gives an action's effect in a state. ```T(S, a, S’)``` defines a transition ```T``` where being in state ```S``` and taking an action ```a``` will take us to state ```S'``` (```S``` and ```S'``` may be the same.) For on-deterministic actions, we also define a probability ```P(S'|S,a)``` which represents the probability of reaching a state ```S'``` if action ```a``` is taken in state ```S```

### Example: Restaraunt
