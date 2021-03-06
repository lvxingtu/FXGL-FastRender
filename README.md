# FXGL-FastRender

A quick demo of various approaches of rendering a single pixel (`fillRect(x, y, 1, 1)`) inside an [FXGL](https://github.com/AlmasB/FXGL) environment. There are not many online resources that are easily available to refer to when it comes to JavaFX drawing performance. Hence, the aim of this demo is to help identify the fastest practical approach to draw a bunch of unrelated pixels (e.g. particles), specifically in JavaFX.

Note that the approaches are not tested directly (e.g. testing if calling `method1()` is faster than calling `method2()`). Hence, approaches like `Canvas.fillRect()` are at a natural disadvantage as they do not support multi-threading and therefore we can see a poor result from `CanvasTestApp`.

It should also be noted that the results below are just a single pass (no comprehensive statistics applied) and they are only for illustrative purposes. Depending on the actual use case (e.g. `drawImage()` or `drawPolygon()`), the results of these approaches might differ.

Finally, if you spot an error in implementation or have suggestions on how to improve performance, contributions are welcome.

![promo](https://raw.githubusercontent.com/AlmasB/git-server/master/storage/images/javafx_render_particles.png)

## Run

```
mvn javafx:run
```

## CPU-based sample tests (how long it took to compute a frame in ms) - 1M particles

- CanvasTestApp

Note: can only run in 1 thread due to Canvas limitations.

```
Avg: 68.7463457
Min: 59.0025
Max: 106.2289
```

- CanvasIntBufferTestApp

Note: using its int buffer for speed _and_ having high-level API makes this the most desirable approach.

```
Avg: 10.3398753
Min: 8.0649
Max: 23.9596
```

- PixelBufferTestApp

Note: there seem to be some flickering issues if fps is low. In addition, no high-level API.

```
Avg: 10.5120373
Min: 7.818
Max: 21.9998
```

- AWTImageTestApp

Note: better performance than high-level Canvas but slower than Canvas with IntBuffer.

```
Avg: 45.6501834
Min: 41.524
Max: 78.3333
```

## GPU-based Sample Tests - 10M particles

- PixelBufferGPUTest

(GPU needs to support OpenCL 1.2 / 2 and have appropriate drivers installed).

Note: we can see that with GPU we can run 10M particles and still beat CPU at 1M.

```
Avg: 8.81632675
Min: 6.1725
Max: 11.0581
```