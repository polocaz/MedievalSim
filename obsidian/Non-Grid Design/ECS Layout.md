## Entities
#### Unit
- Base entity representing a single soldier in the simulation
- Inherits from `AActor`
- Contains unique identifier
- Components attached based on unit type (melee/ranged)
- Responsible for component initialization

#### ECS Manager
- Central coordinator for the ECS architecture
- Inherits from `AActor`
- Responsibilities:
    - System registration and execution order
    - Entity spawning and lifecycle management
    - Component registration
    - Main update loop execution
```cpp
void FECSManager::Update(float DeltaTime) {
    for (FBaseSystem* System : Systems) {
        System->Process(DeltaTime, Entities);
    }
}
```


---
## Components
### Transform Component
- Position, rotation, velocity
- Required by all units
- Used for movement and spatial queries
### Health Component
- Current and maximum health
- Armor value
- Damage resistance
- Death state
### Combat Stats Component
- Attack power
- Attack speed
- Critical hit chance
- Range
- Stamina/Energy
### AI State Component
- Current action being performed
- Target entity ID
- Action cooldowns
- State observations for ML model
- Last action taken
- Action history
### Melee Component
- Melee attack range
- Block strength
- Dodge cooldown
- Block cooldown
- Current stance (normal/blocking/dodging)
### Ranged Component
- Ammunition count
- Reload time
- Projectile speed
- Accuracy
- Minimum engagement range
- Mode switch cooldown (for transitioning to melee)
### Team Component
- Team identifier
- References to nearby allies
- Formation position
- Role within team
- Coordination flags
### Target Selection Component
- Current target
- Potential targets list
- Target priority scores
- Line of sight information
- Engagement status

--- 
## Systems
### ML System (Highest Priority)
- Processes AI decisions
- Gathers observations
- Executes model predictions
- Updates AI state components
### Target Selection System
- Updates potential targets
- Calculates target priorities
- Handles target acquisition and loss
- Manages line of sight checks
### Team Coordination System
- Updates nearby ally information
- Manages formation positioning
- Handles role assignments
- Processes team-wide decisions
### Combat System
- Processes attacks and damage
- Handles blocking and dodging
- Manages combat cooldowns
- Processes mode switching for ranged units
- Validates attack ranges and conditions
### Movement System
- Updates positions and rotations
- Handles collision avoidance
- Processes formation movement
- Manages dodge animations/movement
### Animation System (Lowest Priority)
- Updates visual representations
- Handles combat animations
- Processes movement animations
- Manages state transitions
## Subsystems and Helpers
### Mode Switching Manager
- Part of Combat System
- Handles ranged to melee transitions
- Manages equipment changes
- Updates relevant components
### Formation Manager
- Part of Team Coordination System
- Calculates optimal positions
- Maintains team formations
- Adjusts for terrain/obstacles
## Execution Order
1. ML System (Decision Making)
2. Target Selection System
3. Team Coordination System
4. Combat System
5. Movement System
6. Animation System
## Component Dependencies
- Combat System requires:
    - Health Component
    - Combat Stats Component
    - Either Melee or Ranged Component
    - Target Selection Component
- Movement System requires:
    - Transform Component
    - AI State Component
- Team Coordination System requires:
    - Team Component
    - Transform Component
## Data Flow
1. ML System gathers observations and makes decisions
2. Decisions flow to AI State Component
3. Other systems react to AI State changes
4. Component states update based on system processing
5. New state feeds back into ML observations
## Performance Considerations
- Cache component data in contiguous memory
- Batch process similar components
- Use spatial partitioning for nearby ally/enemy queries
- Minimize cross-system dependencies
- Use events for important state changes
## Extension Points

- New unit types can be added by creating new component combinations
- Additional systems can be registered with ECS Manager
- Component data can be extended without modifying systems
- New ML observations can be added to AI State Component