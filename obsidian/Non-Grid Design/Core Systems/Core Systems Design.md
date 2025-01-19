## 2D Movement System with 3D Rendering
### Define Movement Constraints
Movement for agents will be restricted to a 2D plane, where:
Positions are represented using FVector2D (Unreal’s 2D vector structure).
The Z-coordinate will remain fixed for rendering purposes.
### Agent Representation
Create simple cylindrical 3D models for agents.
Use Unreal’s Basic Shapes or custom low-poly assets.
Cylinders will be scaled to represent height but restricted to 2D movement for simplicity.
### Movement Logic
Create a `MovementComponent` for agents:
Inputs: Target position (`FVector2D`) and current position.
Logic: Use basic vector math to calculate direction and apply speed to move toward the target.
Update loop:
```cpp
FVector2D Direction = TargetPosition - CurrentPosition;
Direction.Normalize();
CurrentPosition += Direction * Speed * DeltaTime;
```
### Rendering Agents in 3D
Convert FVector2D to `FVector` for rendering:
```cpp
FVector RenderPosition(CurrentPosition.X, CurrentPosition.Y, FixedHeight);
AgentMesh->SetWorldLocation(RenderPosition);
```

## Navigation System (`NavMesh` and Pathfinding)
### Setup Navigation Mesh
Use Unreal Engine’s `NavMesh` to create a navigation area:
Add a `NavMeshBoundsVolume` to the scene and scale it to the simulation area.
Configure `NavMesh` settings for 2D navigation by setting a small vertical height tolerance.
### Dynamic Pathfinding
Use Unreal’s `AIController` with `MoveToLocation`:
Attach an `AIController` to each agent.
Use `MoveToLocation` with a 2D target:
```cpp
FVector3D TargetPosition(TargetX, TargetY, FixedHeight);
AIController->MoveToLocation(TargetPosition);
```
### Adjust for Obstacles and Cover
Add `NavModifiers` to represent obstacles and areas with special movement costs (e.g., terrain or cover).
Use Unreal’s Dynamic Obstacle Component for moving objects.
## Entity Component System (ECS Framework)
### ECS Overview
Entity: Represents an agent.
Components: Define modular data for entities (e.g., Position, Health, Speed).
Systems: Perform operations on sets of components.
### Implement ECS Framework
Entity Manager:
Create an Entity class to store unique IDs.
Maintain a list of active entities.

Component Manager:
Use a TMap<EntityID, Component> to store components for each entity.
Example: `TMap<int32, FMovementComponent>`.

Systems:
Define Systems to process batches of components:
- MovementSystem: Updates positions of all entities with a MovementComponent.
- CombatSystem: Updates entities with CombatComponent.
### Example Components
PositionComponent

```cpp
struct FPositionComponent {
    FVector2D Position;
};
```
MovementComponent
```cpp
struct FMovementComponent {
    FVector2D Velocity;
    float Speed;
};
```
### Example Systems
MovementSystem

```cpp
void MovementSystem::Update(float DeltaTime) {
    for (auto& Entity : EntitiesWithMovementComponents) {
        FMovementComponent& Movement = GetMovementComponent(Entity);
        FPositionComponent& Position = GetPositionComponent(Entity);
        
        Position.Position += Movement.Velocity * Movement.Speed * DeltaTime;
    }
}
```
RenderSystem

```cpp
void RenderSystem::Update() {
    for (auto& Entity : EntitiesWithPositionComponents) {
        FPositionComponent& Position = GetPositionComponent(Entity);
        FVector RenderPosition(Position.Position.X, Position.Position.Y, FixedHeight);
        AgentMesh->SetWorldLocation(RenderPosition);
    }
}
```
## Integration Steps
### Create Base Agent Class
Create a `BaseAgent` class inheriting from `AActor`.
Attach the necessary components (e.g., UStaticMeshComponent for rendering, custom ECS components for logic).
Initialize the EntityID and assign components in the constructor.
### Debugging and Visualization
Enable debug visuals:
Use DrawDebugLine to show movement paths.
Display current agent states (e.g., Idle, Moving) using on-screen text.
### Testing
Spawn agents in the world:
Assign random targets within a defined 2D plane.
Observe movement, rendering, and interaction with the NavMesh.