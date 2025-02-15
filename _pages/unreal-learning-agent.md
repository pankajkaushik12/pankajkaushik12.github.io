---
title: Setting Up Learning Agents in Unreal Engine 5
permalink: /unreal-learning-agent
layout: single
author_profile: false
---

## Introduction
Unreal Engine 5 provides a powerful **Learning Agents** plugin that enables AI-driven agents to learn and interact with their environment. This guide walks through the essential steps to set up a learning agent, define its behaviors, and create a training environment.

## Enabling Learning Agents
The first step is to enable the **Learning Agents** plugin in Unreal Engine 5. Once enabled, you can create and manage learning agents within your project.

## Creating an Agent
Any `UObject` can function as an **Agent**. To manage agents efficiently, Unreal provides the **LearningAgentsManager** actor component. Follow these steps:

1. Add `LearningAgentsManager` as a component to a child of an `Actor` class.
2. This manager stores references to agents and implements agent logic.
3. Extend its capabilities by adding components such as:
   - **Interactor** (for managing agent interaction)
   - **Trainer** (for training workflows)
   - **Recorder** (for logging and tracking agent performance)
4. Place the `LearningAgentsManager` actor in the level.

Next, agents need to be added to the manager using the `AddAgent` node in Blueprint.

> **Debugging Tip**: To view warnings and errors, search for `LogLearning` in the output log window.

## Defining Agent Behavior
To specify how the agent interacts with its environment, create a child class of `LearningAgentsInteractor` and override the following functions:

- **`SpecifyAgentObservation`**: Defines the structure of the agent's inputs.
- **`SpecifyAgentAction`**: Defines the structure of the agent's outputs.
- **`GatherAgentObservation`**: Collects observation data from the environment.
- **`PerformAgentAction`**: Executes the agent's actions in the environment.

The first two functions define the input-output structure of the agent's policy, while the latter two manage data collection and action execution.

## Creating a Training Environment
To train an agent, you need a suitable training environment. Create a child class of `LearningAgentsTrainingEnvironment` and override these functions:

- **`GatherAgentReward`**: Determines the reward value based on agent performance.
- **`GatherAgentCompletion`**: Checks if the training episode should terminate.
- **`ResetAgentEpisode`**: Resets the environment and agent for a new training episode.

## Setting Up Training Components
To train the agent using reinforcement learning, create a **DataAsset** for the following:
- **Encoder** (to process observations)
- **Decoder** (to process outputs)
- **Critic** (to evaluate actions)
- **Policy** (to define agent decision-making)

At `BeginPlay` of the `LearningAgentsManager`, follow this sequence:

1. **Add the Interactor**
   - Use the `MakeInteractor` node to add the custom `LearningAgentsInteractor` class created earlier.

2. **Set Up the Policy**
   - Use the `MakePolicy` node to connect the Interactor and assign the Neural Network assets for the Encoder, Decoder, and Policy.

3. **Define Training Environment**
   - Use the `MakeTrainingEnvironment` node to add the Critic `DataAsset` and the custom training environment class.

4. **Initialize Training**
   - Use the `MakePPOTrainer` node to set up the reinforcement learning trainer.

## Conclusion
By following these steps, you can successfully set up and train a learning agent in Unreal Engine 5. Experiment with different policies and environments to enhance your AI-driven simulations!

## Additional Resources
For further learning, refer to the official Unreal Engine Learning Agents course: [Unreal Engine Learning to Drive 5.5](https://dev.epicgames.com/community/learning/courses/GAR/unreal-engine-learning-agents-5-5/7dmy/unreal-engine-learning-to-drive-5-5).