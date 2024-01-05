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
2. Take the center point of each generated room and connect them using the Greedy algorithm
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
- Control over the layout
- Logical corridor connections (If using greedy algorithms)

**Disadvantages:**
- 'potentially' less natural <-- can be resolved with Delaunay Triangulation
- Complexity
- 'Possibly' repetitive <-- can also be resolved with Delaunay Triangulation

## Detailed explanation of each step
## Corridor-first procedural generation
**1. Create corridors using the random walk algorithm**

https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/7a0018fe-3d10-4130-b503-69e1d40aef89

The video above shows us the bare corridors it generates from using the random walk algorithm
we have 2 parameters here
* Corridor length (How long is each corridor?)
* Corridor count (How many corridors do we generate?)

Let's take a closer look at the Random walk algorithm applied on the corridor

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/d35289f2-31d3-4a31-af5f-99fb37512699)

We have a blank grid, let's paint the starting point of our corridor in red.

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/2dd58064-b4a6-4742-ae78-971b3fb8f070)

great! we now start from the start point to any of the cardinal directions, the length is defined beforehand as a parameter, let's take corridor length 5 for example

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/9b31318f-9bb2-4591-94d2-27f1e492c7ec)

It has completed its first corridor count going in the right direction, we repeat this until we have done exactly the amount of corridors we wanted, which is our 2nd parameter, let's take corridor count 5 as well.

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/fa60ce2f-f01f-4b73-8f6f-808602af8d4e)

After finishing the loop of the inputted amount of corridors, the generated corridor might look like this.

Now, it is important to note that a random walk might cause it to decide to go back in the direction it came from, and thus seemingly make it only have 4 corridors while it just overlapped the same position it went before

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/832a5019-5a03-4190-ba98-d3d9496c5e94)

Aside from that exception, this concludes the random walk algorithm applied to the corridor

**2. generate rooms at random points of the corridor also using the random walk algorithm**

https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/805b47bb-4648-42fb-916f-42e26fa8c060

After creating the corridors, there will be a set % chance between 10 and 100% to generate rooms in the endpoints of each corridor made. 
The rooms are generated using the same algorithm, but they can be generated with different algorithms or variations of the random walk, i will show you 2 different ways of random walk-generated rooms I have implemented

### 1. Random walk Island
With this algorithm, the rooms are more island-shaped and smaller concentrated around the generating point
Here, we start from the origin of the room in every iteration.

https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/bf929cfe-8986-410c-aaf5-d9a17dc278e1


### 2. True random walk
This algorithm will continue the random walk where it ended its iterations, making the rooms more of a "cavern" feel or spacious rooms.

https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/1a914273-770a-479e-8631-8a4925f6f75f

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/34ac6619-ae60-4d80-bca8-7ea6323fa21d)

We can easily add different instances of room generations, the simple parameters do the following:
* Iterations (How many times do we repeat the Random Walk?)
* Walk Length (How many tiles do we move in any cardinal direction in one iteration?)
* Start randomly each iteration (Does the random walk start back to the origin every iteration or not? If not it keeps continuing where it ended its iteration

**3. generate rooms at each dead end of the corridor using the same algorithm**

You may or may not have noticed that this generation method has a possibility of having dead ends, which can be unwanted. I resolved this by creating rooms at each dead end the same way as generating rooms. But did I detect those dead ends you may ask?
Well, it's quite simple! I first check each floor tile for its neighbors, and tiles that only have 1 neighboring floor tile is a dead-end tile, if that is the case I add it to the list of dead-ends that need a room generated.

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/105a78e4-527d-4fc4-9818-7cee47d81533)

In code:

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/0a5f6d39-c1d0-49e5-91ac-c6f5edafad7a)

**4. Place the wall tiles to finish the look**
Now this step is a little trickier, but basically, it uses the same logic on how to detect dead ends
Each tile is being checked on its neighboring tiles on whether they are floortiles or not, and based on the binary combination it has a unique binary set in which we can assign the correct tile texture

To illustrate:

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/bd471c3a-8cb5-4e47-878c-6ec686a10397)

Here we check each wall tile on its neighboring tiles on whether they are floortiles or not in Clockwise order, it returns a Binary set of numbers in which we can find the corresponding wall type it should be (in this case a full wall)
Here's another example

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/1a80d4f6-500c-4802-a422-fd4299f43e76)

And this finishes our corridor-first procedural dungeon generation.

## Corridor-first procedural generation
**1. Generate rooms using Binary Space Partitioning (BSP) or random walk**



# Conclusion and where to go next
This project has given me the opportunity to research the procedural generation world and I was fascinated by the very broad possibilities and flexibility this area has!
With this research topic having given me a better fundamental understanding of how it works, there is still a lot of room for improvements or expansions

- Adding Delaunay triangulation to create corridors (Very recommended)
- Procedurally generating props/items or enemies in the procedurally generated dungeons
- 3D procedural generation
- Optimizing the generations, for example: currently, BSP room generation runs on O(n²) time
- ...

In conclusion, this 2D Dungeon generator has lots of potential and can be a powerful tool for game designers with further optimization and development

# References
- Random walk algorithm (https://en.wikipedia.org/wiki/Random_walk)
- Binary Space Partitioning (https://en.wikipedia.org/wiki/Binary_space_partitioning)
- Procedural Dungeon Theory (https://youtu.be/jlNqp_QAibo?list=PLcRSafycjWFenI87z7uZHFv6cUG2Tzu9v)
- Procedural generation in Unity (https://youtu.be/PhLcNhK9aro)
- Procedural Dungeon using Random walk (https://blog.jrheard.com/procedural-dungeon-generation-drunkards-walk-in-clojurescript)


