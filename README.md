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

Now, number 3 might be a bit confusing, but essentially I see this project as a long-term project and want to utilize its potential for my future games or further research, i will discuss this a bit later

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

### Room-first procedural generation
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

### A. Random walk Island
With this algorithm, the rooms are more island-shaped and smaller concentrated around the generating point
Here, we start from the origin of the room in every iteration.

https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/bf929cfe-8986-410c-aaf5-d9a17dc278e1


### B. True random walk
This algorithm will continue the random walk where it ended its iterations, making the rooms more of a "cavern" feel or spacious rooms.

https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/1a914273-770a-479e-8631-8a4925f6f75f

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/34ac6619-ae60-4d80-bca8-7ea6323fa21d)

We can easily add different instances of room generations, the simple parameters do the following:
* Iterations (How many times do we repeat the Random Walk?)
* Walk Length (How many tiles do we move in any cardinal direction in one iteration?)
* Start randomly each iteration (Does the random walk start back to the origin every iteration or not? If not it keeps continuing where it ended its iteration

**3. generate rooms at each dead end of the corridor using the same algorithm**

You may or may not have noticed that this generation method has a possibility of having dead ends, which can be unwanted. I resolved this by creating rooms at each dead end the same way as generating rooms. But did I detect those dead ends you may ask?
Well, it's quite simple! I first check each floor tile for its neighbors, and tiles that only have 1 neighbouring floor tile is a dead-end tile, if that is the case I add it to the list of dead-ends that need a room generated.

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/105a78e4-527d-4fc4-9818-7cee47d81533)

In code:

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/0a5f6d39-c1d0-49e5-91ac-c6f5edafad7a)

**4. Place the wall tiles to finish the look**
Now this step is a little trickier, but basically, it uses the same logic on how to detect dead ends
Each tile is being checked on its neighbouring tiles on whether they are floortiles or not, and based on the binary combination it has a unique binary set in which we can assign the correct tile texture

To illustrate:

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/bd471c3a-8cb5-4e47-878c-6ec686a10397)

Here we check each wall tile on its neighbouring tiles on whether they are floortiles or not in Clockwise order, it returns a Binary set of numbers in which we can find the corresponding wall type it should be (in this case a full wall)
Here's another example

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/1a80d4f6-500c-4802-a422-fd4299f43e76)

And this finishes our corridor-first procedural dungeon generation.

## Room-first procedural generation
**1. Generate rooms using Binary Space Partitioning (BSP) or random walk**

https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/81918fa0-e86a-4c11-9603-a9d0aefcce2b

In this generation method, rooms will be generated first, they can either be done with Random walk like before, but I wanted to get square or rectangular rooms as well, after browsing and comparing several 2D dungeon generation methods,
I found out that many use a technique called BSP (Binary Space Partitioning), let's take a simple rundown of how it works

First, we give it a total dungeon size, this will be the main field we will cut up

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/2403df22-e53c-4f5e-8b5a-99c8d4accb28)

Now with our other parameters, namely the minimum room width and minimum room height, we have set a minimum size of our rooms and an offset, which will decide how much space is divided between the rooms.
Now BSP will randomly decide points to cut our main dungeon size

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/e00f1f9a-3906-47a1-b8db-2479ae99f9de)

We now have 2 rooms, but we will keep going until we cannot create any more rooms due to our minimum sizes not being achieved with the offset taken into account
In the image below, we no longer see room 2, because room 2 is now the combination of both rooms 3 and 4. This is a very simplified explanation of how BSP works

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/d1c9c2b3-fd3c-4904-858c-72f911ebb67c)

**2. Take the center point of each generated room and connect them using the Greedy algorithm**

Great, now that we have rooms generated all that's left is to connect them, now I would like to point out that this is the area I want to improve upon since I'm using a very easy algorithm and there are better ones such as the Delaunay Triangulation.
But I'm still new to creating my own procedural generation, so I decided it's best to stick with simpler concepts first before attempting more complex ones, so I opted for Greedy Algorithms to pave the way for us.

Greedy algorithms are of course greedy! They aim for the goal (destination) in the shortest amount of steps.

Firstly I made each room have its own center point which is our 'potential' destination.

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/2e138cac-fbcb-4c42-bb4c-c312a5bf49b5)

Next up is deciding a random starting point in one of the centers of any room, from there we search for the closest center point of any room to the starting point and use that point as its destination goal. the algorithm will create floor tiles along the way towards that destination.

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/941dcc9c-9295-4022-af5e-591ef109a641)

After reaching the destination, it searches if there are still some more points it needs to reach if there are, it picks the closest to its current position once more and travels to that point.

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/9c500fbd-76c6-46fe-ab01-21d4d9d4fe59)

This process is repeated until there are no more destinations to be reached, in the end, the corridor layout may look like this:

![image](https://github.com/GLWMaxCOD/GPP_ResearchProject_ProceduralGeneration/assets/62150523/6a73c63c-de06-4547-8551-e7ad0c69e321)

Okay, now you can notice the potential risk of getting "linear" levels, this is why I want to improve upon this area the most, if you want as a game designer to have more random interconnected rooms, having Delaunay triangulation will solve this problem.

**3. Place the wall tiles to finish the look**

This step is the same as the corridor first approach.

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


