## Pixel Sampling

Pixel sampling is the process of using the pixels of one image to
color the pixels of another image, by sampling the pixels of the
former image. In the process of texture mapping, there is an object
(like a triangle) which is being rasterized, and there is an image
known as the *texture* which provides the pixels that will be colored
onto the object. In order to find the color of a pixel in the
rasterized version of the object, the texture is sampled at a
specified location, either using *nearest* or *bilinear* sampling, and
then the sampled color is painted onto the rasterized pixel.

In order to do pixel sampling on a triangle, first we obtain the
barycentric coordinates of the pixel in the triangle, then we use the
barycentric coordinates to find the correct location to sample the
texture from a triangular region whose vertices are given to us.

In **nearest sampling**, the pixel in the texture nearest to the sampling
location is chosen, and its color is painted on the triangle pixel.

In **bilinear sampling**, a weighted average of the four texture
pixels surrounding the sampling location is calculated. The weighting
is based on the closeness of each texture pixel to the sampling
location. This weighted average color is painted on the triangle
pixel.

### Comparison of Bilinear vs. Nearest Pixel Sampling

The four images below show a comparison of bilinear and nearest pixel
sampling. As we can see, nearest pixel sampling is more prone to
having rougher edges, and the colors seem somewhat noisier due to
sharp transitions. On the other hand, bilinear sampling creates
smoother gradients of colors, and also makes edges appear smoother
even at lower sampling rates.

![](assets/img/nearest_1pp.png)
![](assets/img/bilinear_1pp.png)
![](assets/img/nearest_16pp.png)
![](assets/img/bilinear_16pp.png)

We believe that there will be a large difference between the
performance of these two sampling algorithms when there is a large
degree of texture minification, where nearest pixel sampling will be
prone to aliasing more than bilinear sampling, since bilinear sampling
is equivalent to low-pass filtering after taking extra samples, which
leads to antialiasing.
