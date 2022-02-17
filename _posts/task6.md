## Part 6: Level sampling for texture mapping

We level sample to figure out the correct resolution of textures to
use in our image. To write this helper function, we first calculate
($\frac{\partial u}{\partial x}, \frac{\partial v}{\partial x})$ and
$(\frac{\partial u}{\partial y}, \frac{\partial v}{\partial y})$ by
looking at the barycentric coordinates of adjacent points $P(x,y),
X(x+1, y), \text{ and } Y(x, y+1).$ After calculating the
differentials $PX = P - X$ and $PY = P + Y$, we can determine the
level by taking $\log_2 \max \\{\|PX\|, \|PY\|\\}$.

### Tradeoffs

Nearest and zero level sampling have similar speeds, since they only
need to sample a single texture image. Linear level sampling, however,
requires us to sample two images, as well as perform a linear
interpolation step, which takes many more instructions per pixel of
the rendered image, and thus it takes noticeably longer. Zero level
sampling has the most aliasing when texture minification is involved,
linear level sampling is the best since it not only finds an
appropriate resolution to sample the texture at, but also performs
averaging which reduces aliasing, and nearest is somewhere in the
middle where minification artifacts are not prominent, but there is
no antialiasing effect due to averaging of pixel values. Zero level
sampling uses the least amount of memory, since a single mip needs to
be stored, whereas several layers of mips need to be stored for the
other two level sampling methods. However, the extra memory overhead
is not too bad since the extra images are of lower resolution, and
indeed only a fraction extra memory is used. There may however be
slowdowns due to increased cache misses especially in linear level
sampling, where we need to access different mip maps stored in
relatively faraway memory locations in a single function call.

Below we show the differences of the required combinations of level
and pixel sampling methods. As we can see, with a 1 pixel
supersampling rate, level zero sampling leads to worse aliasing.
Nearest level sampling leads to lower minification artifacts. Bilinear
pixel sampling reduces aliasing compared to nearest pixel sampling.

![](../assets/proj1_img/task6_img/l0pnearest.png)
![](../assets/proj1_img/task6_img/l0pbili.png)
![](../assets/proj1_img/task6_img/lnearestpbili.png)
![](../assets/proj1_img/task6_img/lnearestpnearest.png)
