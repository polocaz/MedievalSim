The best approach for this project seems to be Reinforcement Learning.

## Key Concepts
The ideal version of this project would contain the following:
1. State space
	1. Units keep track of their own health, position, and cooldowns
		1. Part of the position component would include terrain
	2. Nearby enemies and allies along with their state (blocking, moving, attacking, etc.)
	3. Closest cover
	4. Current weapon state
		1. Most likely will only be used for ranged units to keep track of ammo
	5. Relative position to objectives and teammates
		1. ??? Need to determine cost vs value
2. Action Space
	1. Move in any direction that is not blocked
	2. Attack targets
	3. Block or Dodge
	4. Switch between ranged and melee
	5. Choose positions based on team, terrain, enemies and obstacles
3. Reward structure
	1. Positive
		1. Dealing damage
		2. Successful blocks and dodges
		3. Surviving the battle
		4. Winning/Killing an enemy
		5. Protecting teammates
			1. Need to quantify
		6. Coordinating with team
			1. Also not clear how to quantify
	2. Negative
		1. Receiving damage
		2. Harming allies (ranged only?)
		3. Dying
		4. Leaving formations assigned

## Reward function

Proximal Policy Optimization:
- Relatively simple to implement
- Stable during training
- Works well for continuous action spaces
- Good for multi-agent scenarios
## Flow

During each tick we will do the following:
1. State collection
	1. The ML System will iterate through all units and collect state from the different components
	2. A tensor is built out of the unit states and passed into our neural network
2. Decision making
	1. Network outputs probabilities for possible actions
		1. How are potential new positions calculated?
	2. Highest probability action will be chosen?
3. Action execution
	1. For each unit, the components will be updated based on the action chosen.
		1. Updates component values like velocity, combat state, and targets
	2. The updated component values will then be processed by the other systems 
		1. Movement processes velocity and target
		2. Combat processes combat state and targets