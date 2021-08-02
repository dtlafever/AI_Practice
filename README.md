# AI_Practice

Following and building off the tutorial: https://www.unrealengine.com/en-US/onlinelearning-courses/introduction-to-ai-with-blueprints

Made in Unreal Engine 5

Further Documenation from Epic Games can be found here: https://docs.unrealengine.com/4.26/en-US/InteractiveExperiences/ArtificialIntelligence/

## Description

This project is a 3rd person game where an AI will follow a player relatively close, eat food at designated locations when hungry, and wander around the map randomly if no player is in sight. This focuses on learning the basics of Behavior Trees and how they interact with Blackboards, as well as the experimental Environment Query System or EQS for the following behavior and food system. Additionally, there is the Test Pawn to help us debug the EQS system. This is in no way a full game demo. This project was simply to understand the basics of AI and follow along the aformentioned tutorial provided by Epic Games.

## Potential Next Steps

- Adding more senses (i.e. hearing)
  - Adjust priorities to make the existing sight sense the higher priority
- Expand the AI's sense of the Environment
  - Use navigation links to have the AI jump down from the raised walkway rather than walk all the way around
- Add additional tests to the EQS queries to make them more refined
  - Add a dot product test to the existing find hide spot query. This could be used to shape the points on the ground to face towards a particular direction. An example of this would be esuring that the AI only hid in front of the player more like a cover query.
- Simple Parallels (Think)
  - Check that the AI is in range for a melee attack while still moving towards the player
- Decorator types
  - add conditional loops and cooldowns to manipulate the flow of execution throughout the behavior tree.
- Improve wandering routine to be more like a search routine
- create a patrol system

## Notes

*Sense -> Think -> Act -> Loop*

Press "apostrophe" on the keyboard during gameplay to bring up the gameplay debugger. This is a very useful tool for debugging AI systems.

### Important Editor Settings

- Update Navigation Automatically
  - By default it is enabled. Disable it if you are working on a very large map as this can reduce performance in editor significantly. To update this manually, go to *Build->Build Paths*.

### Important Project Settings

- Navigation System
  - Under the *Agents* header, you can add types of Agents to allow for default values for ground vs air units, different sized units, etc.
- Navigation Mesh

### Important Actors/Objects in scene

- NavMesh Bounds Volume
  - Used for generating navmesh walkable areas
  	- Set the size using *Brush* settings
- Recast NavMesh
  - Generated when you create NavMeshBoundsVolume. allows tweeking of various values found in the Navigation System such as how large and tall your agents are expected to be. 
- Nav Modifier Volume
  - Creates various volumes to make specific areas impassable by NavMesh, or to increase/decrease cost to enter area.

### Important Components to attach to Characters

- AI Perception
  - Used to perceive various senses such as Hearing or Seeing. You can set what senses the character will have using this component, as well as using functions like *On Target Perceived Updated* to perform an action when a target sensed.
  - Here you can also set *Detection by Affiliation* which allows for detecting Enemies, Neutrals, and Friendlies. Note that by default Neutrals are always detected but enemies and friendlies must be set in C++.
- AI Perception Stimuli Source
  - Used to generate stimuli for an AI perceptor. Can be placed on a player character so the AI can sense the player based on your stimuli source.

### Behavior Trees and Blackboards

- Logic goes in the Behavior Tree
  - Decorators
    - Decorators in a behavior tree node are best described as a question. Such as "Is Target Actor Set?" This makes it more readable and makes sure it is being used correctly.
    - When setting up Decorators, note that *Notify Observer* has two choices: *On Value Change* and *On Result Change*. Changes in value can be changing from one target to another, while result change is the difference between having a target set or none at all.
- Memory goes in the Blackboard

## Ingame explinations

### Environment Query System

- EQS_HideNearPlayer

Generate potential locations near the player and pick ones that are closest to the player controller 0.

Trace: Filter out locations that are on top of player
Distance: Score better the closer we are to player
PathExist: Filter out and score paths we can actually go to