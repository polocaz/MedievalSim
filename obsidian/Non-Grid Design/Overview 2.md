## **1. Project Summary**

This project is a **3D-visualized combat simulation** where agents are constrained to a **2D plane** for movement and combat. The simulation will feature both **melee agents** and **ranged agents**, each with distinct behaviors and combat styles.

The primary goals are to:

1. Learn **machine learning** by training agents to optimize combat strategies.
2. Build the system in **Unreal Engine**, using **data-oriented design** principles for optimization and scalability.
3. Achieve a balance of performance and realism through efficient processing and physics-based interactions.

---

## **2. Key Features**

### **2.1. Simulation**

- Agents move and interact within a **2D plane** but are rendered in **3D** for enhanced visualization.
- Supports dynamic environments with obstacles, cover, and varied terrain effects.

### **2.2. Agent Types**

- **Melee Agents:**
    - Engage enemies at close range.
    - Actions include attacking, dodging, blocking, and positioning for optimal combat.
- **Ranged Agents:**
    - Attack from a distance using projectiles.
    - Actions include shooting, taking cover, and retreating to maintain distance.

### **2.3. Physics and Combat**

- Physics-based interactions like knockbacks, collision-based attacks, and projectile trajectories.
- Line-of-sight calculations for ranged combat to ensure realistic target selection.

### **2.4. Machine Learning**

- Agents will use **Reinforcement Learning (RL)** to learn:
    - Optimal movement paths.
    - Combat strategies (e.g., melee flanking or ranged kiting).
    - Tactical decisions like when to attack, defend, or retreat.

---

## **3. Technical Breakdown**

### **3.1. Movement and Navigation**

- **Constrained 2D Plane:**
    - Agent positions are defined by `FVector2D` for movement but rendered in 3D.
- **Dynamic Navigation:**
    - Use Unreal’s NavMesh for continuous space pathfinding, adjusting for obstacles and cover dynamically.

### **3.2. Combat System**

1. **Melee Combat:**
    
    - Triggered when within attack range of a target.
    - Requires pathfinding to close gaps and maintain optimal positioning.
    - Includes cooldown-based attacks and stamina for special actions like blocks or dodges.
2. **Ranged Combat:**
    
    - Requires line-of-sight validation via raycasting.
    - Projectile physics modeled for realistic trajectories.
    - Focus on maintaining distance from melee threats and finding cover when targeted.
3. **Cover System:**
    
    - Identify and utilize obstacles for tactical positioning.
    - Ranged units prioritize cover when attacked by melee enemies or ranged threats.

### **3.3. Agent Decision-Making**

- **Behavior Trees:**
    - Manage core actions like idle, move, attack, and defend.
- **AI Perception System:**
    - Detect nearby allies and enemies using sight and sound.
- **Reinforcement Learning Integration:**
    - Train agents to optimize rewards like damage dealt, survival time, and strategic positioning.

### **3.4. Rendering and Visualization**

- Agents represented as **cylindrical 3D models**.
- Minimal visual complexity for performance while allowing debug overlays (e.g., attack ranges, perception radii).

---

## **4. Data-Oriented Design Approach**

### **4.1. ECS Architecture**

- **Entities:** Represent agents as data containers.
- **Components:** Modular blocks of data (e.g., position, health, combat stats).
- **Systems:** Perform operations on groups of components (e.g., movement, combat updates).

### **4.2. Batch Processing**

- Process similar actions (e.g., all movement updates, AI decisions) in a single step to minimize CPU cache misses.

---

## **5. Machine Learning Pipeline**

1. **Observation Space:**
    
    - Agent state (position, health, stamina).
    - Nearby entities (allies, enemies, obstacles).
    - Environmental data (cover locations, terrain costs).
2. **Action Space:**
    
    - Continuous movement directions.
    - Discrete actions (attack, defend, retreat).
3. **Reward System:**
    
    - Damage dealt, survival time, and tactical actions like utilizing cover or retreating effectively.
4. **Integration Workflow:**
    
    - Train RL models externally using frameworks like **PyTorch**.
    - Import models into Unreal for testing and deployment.

---

## **6. Development Plan**

### **6.1. Phase 1: Core Systems**

- Set up the 2D movement system with 3D rendering.
- Implement NavMesh and pathfinding logic.
- Build a simple ECS framework for agents.

### **6.2. Phase 2: Combat and AI**

- Develop melee and ranged combat logic.
- Integrate AI Perception for agent awareness.
- Prototype behavior trees for basic decision-making.

### **6.3. Phase 3: Machine Learning**

- Create data logging for training simulations.
- Train RL models and integrate them into Unreal.
- Refine AI behaviors based on performance metrics.

### **6.4. Phase 4: Optimization and Debugging**

- Optimize batch processing for large-scale simulations.
- Add visualization tools for debugging (e.g., AI states, paths).
- Polish and finalize the simulation.