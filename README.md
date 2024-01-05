# 2D Procedural Dungeon generator

### A 2-dimensional procedural dungeon generator that allows us to generate dungeons!

# Motivation

I have always been fascinated by games using procedural generation to make their games feel new every time you play them, ranging from complex cave generation (Deep Rock Galatic) to Minecraft's 'endless' voxel world generation to simpler ones such as the dungeon generation from Unbinding of Isaac, and a recent game called Lethal Company that took the world by storm from a singular dev!

Clearly, games that use procedural generation have long-lasting lifetimes and fun, because the maps you play on them never are the same, making it perfect to replay them over and over. With my current education at DAE Howest, this was one of my goals to learn and thankfully, the course Gameplay Programming provided me with an opportunity to research a topic, so I took the chance and studied this topic in detail

# The project's goals

While I know many types of procedural generation and how they work, it is a smart idea to start with the basics, 2D Dungeon generation...
This way, I can learn the fundamental concepts and techniques of the procedural generation technique. Before starting this project I set some goals for myself to achieve:

1. Create a dungeon generator with a customization interface where I can adjust various parameters from dungeon size to the number of rooms.
2. Use the dungeon generator with a tileset to generate nice-looking rooms
3. Allow the code/algorithm to be open-ended, so I can use it to eventually add more complex algorithm possibilities
4. Understand how procedural generation works

Now, number 3 might be a bit confusing, but essentially i see this project as a long-term project and want to utilize its potential for my future games or further research, i will discuss this a bit later

# Final Result

### Corridor-first procedural generation
https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/5586ea8a-5195-4e30-b12e-6bcc04f0e8c7

### Room-first procedural generation
https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/a0e94d86-ed67-4880-99ee-4f9e567c9df9

# How it works

### Corridor-first procedural generation
1. Create corridors using the random walk algorithm
2. generate rooms at random points of the corridor also using the random walk algorithm
3. generate rooms at each dead end of the corridor using the same algorithm
4. Place the wall tiles to finish the look

### Room-first procedural generation
1. Generate rooms using Binary Space Partitioning (BSP) or random walk
2. Take the center-point of each generated room and connect them using Greedy algorithm
3. Place the wall tiles to finish the look

## Advantages and disadvantages of each generation
Now, both generations have their advantages and disadvantages

### Corridor-first procedural generation
**Advantages:**
- Natural layouts
- Simplicity
- (Dead End rooms) if wanted
- Ease of navigation

**Disadvantages:**
- Limited room variation
- Less structured
- Less control

### Corridor-first procedural generation
**Advantages:**
- Structured layout
- Variety in room sizes (if using BSP)
- Control over layout
- Logical corridor connections (If using greedy algorithms)

**Disadvantages:**
- 'potentially' less natural <-- can be resolved with Delauney Triangulation
- Complexity
- 'Possibly' repitive <-- can also be resolved with Delauney Triangulation

## Detailed explanation of each step
### Corridor-first procedural generation
**1. Create corridors using the random walk algorithm**

