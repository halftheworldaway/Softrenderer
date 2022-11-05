# SoftRenderer
在不使用图形API，像是OpenGL，DirectX，Vulcan的情况下，通过简单的画点函数和SDL2所提供的创建窗口功能来实现的简易软光栅渲染器

## 当前进度
  ### 1. 正确的通过屏幕坐标来光栅化三角形和重心坐标插值进行顶点着色
  ![](/asset/Triangle.png)
  
  本项目中光栅化算法用的并不是CPU软光栅常用的扫描线算法，而是模拟GPU的简单暴力策略，即先求出整个三角形的AABB包围盒，然后测试包围盒的中的每个像素是不是在三角形中，如果在的话，就根据重心坐标来进行颜色插值，然后将像素值存入帧缓存(frame buffer)中。

  ### 2. 通过Z-buffer来正确的渲染旋转的正方体
  ![](/asset/depth_buffer.gif)
  
  相比于传统的画家算法将物体在深度上进行排序，然后从远及近的渲染，Z-buffer通过一种空间换时间的思维，在光栅化时不断的记录每个像素点对应的深度值，并且只有当正在画的像素值的深度小于Z-buffer中记录的深度时，我们才将其进行绘制。(因为本项目在各个坐标系下都是采用左手坐标系，所以Z值越小代表离我们越近)。

  ### 3. 简单光照
  ![](/asset/mylight.gif)
  
  在片元（像素）着色器实现了一个简单的光照模型，关键在于注意将法线变换到世界坐标系时不能简单的乘以模型矩阵，而是乘以模型矩阵逆矩阵的转置，只有这样我们才能保证模型如果发生了不等比缩放，我们的法线可以依旧垂直于平面切线。
