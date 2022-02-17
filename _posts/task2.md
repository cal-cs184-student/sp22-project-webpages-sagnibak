### Part 2: Antialiasing triangles 

To properly antialias, we resized the sample buffer by `sqrt{sampling_rate}` in both height and width dimensions. To avoid any potential floating point issues, we opted to scale our triangle up and traverse through the triangle in whole increments as opposed to keeping the triangle the same size and incrementing the traversals in fractional amounts. In the event that the point lies inside the triangle, we update the `sample buffer` to the color. Then, to render the image at the original size, in `resolve_to_framebuffer`, we take the average of `sampling rate` number of samples (a square that corresponds to one pixel) and update the `rgb_framebuffer` (the actual image rendering). This antialiases the triangles by providing inbetween colors. This can be seen as applying a filter ovr a pixel getting closer and closer to the true average color of the pixel. 

<div align="middle">
  <table style="width=100%">
    <tr>
      <td>
        <img src="https://github.com/cal-cs184-student/sp22-project-webpages-sagnibak/tree/master/assets/proj1_img/task2_img/task2_1.png" align="middle" width="400px"/>
        <figcaption align="middle">Sample rate: 1.</figcaption>
      </td>
      <td>
        <img src="https://github.com/cal-cs184-student/sp22-project-webpages-sagnibak/tree/master/assets/proj1_img/task2_img/task2_4.png"align="middle" width="400px"/>
        <figcaption align="middle">Sample rate: 4.</figcaption>
      </td>
       <td>
        <img src="https://github.com/cal-cs184-student/sp22-project-webpages-sagnibak/tree/master/assets/proj1_img/task2_img/task2_16.png" align="middle" width="400px"/>
        <figcaption align="middle">Sample rate: 16.</figcaption>
      </td>
    </tr>
  </table>
</div>

The difference between sample rate 1 and sample rate 16 can be seen very clearly in the very skinny triangle corner. As we take more samples we can better approximate the true color. When we don't supersample, because the triangle is skinny, probabilistically supersampling will better approximate the pixel. However, in practice, there will be situations where it just so happens that the point you sample ends up being inside the triangle. This is why even when the sample rate is 16, the zoomed in image isn't smooth. 