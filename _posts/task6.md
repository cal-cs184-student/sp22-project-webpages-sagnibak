## Part 6: Level sampling for texture mapping

We level sample to figure out the correct resolution of textures to use in our image. To write this helper function, we first calculate ($\frac{\partial u}{\partial x}, \frac{\partial u}{\partial x})$ and $(\frac{\partial u}{\partial x}, \frac{\partial u}{\partial x})$ by looking at the barycentric coordinates of adjacent points $P(x,y), X(x+1, y), \text{ and } Y(x, y+1).$ After calculating the differentials $PX = P - X$ and $PY = P + Y$, we can determine the level by taking $log_2 max \{\|PX\|, \|PY\|\}$.

Tradeoffs between speed, memory usage, antialiasing 