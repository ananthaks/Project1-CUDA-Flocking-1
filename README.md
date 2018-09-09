Project 1 Flocking
====================

**University of Pennsylvania, CIS 565: GPU Programming and Architecture**

**Anantha Srinivas**
[LinkedIn](https://www.linkedin.com/in/anantha-srinivas-00198958/), [Twitter](https://twitter.com/an2tha)

**Tested on:**
* Windows 10, i7-8700 @ 3.20GHz 16GB, GTX 1080 8097MB (Personal)
* Built for Visual Studio 2017 using the v140 toolkit

Implementation
---

This is the simulation of 100K boids at 350 Frames per second. This shows semi-coherent memory access uniform grid flocking.

![](images/boid9.gif)

---

__Performance Graph__
![](images/performance_graph.PNG)

Experimental setup
---
* Tested on a GTX 1080, with compute capability of 6.1
* NVIDIA vertical Sync was disabled (which actually increased the frame rate).
* Visualization was turned off and program run in Release mode.

Interesting Insights
---
* Even though semi-coherent memory access involves an extra step in making sure that the position and velocity data are contiguous, it does run faster than regular unifrorm grid search method. This can probably be attributed to caching and faster memory access in GPU. 

* I noticed that uniform grid search is not best throughout. Although it does perform way better than brute force searching, the frame rate does not linearly increase withnumber of boid. The frame rate actually peaks somewhere near 10k boids (which is suprisingly faster than 5k boids) suggesting that this method of searching needs to have some level of saturation in the data.

Further thoughts
---
* **For each implementation, how does changing the number of boids affect performance? Why do you think this is?** As a general rule increasing the number of boids decreases the framerate across all methods. However, there were some anomolies as noted above. For uniform and coherent memory access methods, the frame rate actually peaked for 10k boids. The frame rate generally decreases because the number of threads to be launched increases, which further increases the GPU load. As a general rule of thumb only around 60 ~ 80% of GPU should be loaded to get best performance. 

* **For each implementation, how does changing the block count and block size affect performance? Why do you think this is?** This did not affect the performance much. This may be because the default block size of 128 is much smaller than the boid size. Also, block is a logical contruct, so it should technically not matter if all the threads are running the same instructions.

* **For the coherent uniform grid: did you experience any performance improvements with the more coherent uniform grid? Was this the outcome you expected? Why or why not?** Yes, Coherent grid, resulted in a slightly better performance. This could be due to easier/faster memory access to the GPU. It could also possibly cache the data since they are contiguous.

* **Did changing cell width and checking 27 vs 8 neighboring cells affect performance? Why or why not? Be careful: it is insufficient (and possibly incorrect) to say that 27-cell is slower simply because there are more cells to check!** It doesnt matter to a certain extend on what is the cell width. All the boids have to be covered in some iteration.



