title: Shader
author: 大帅
date: 2018-06-11 13:27:11
tags:
---
### 着色器
- LinearGradient 
      线性变色，Shader shader = new LinearGradient(0, 0, 100, 100, Color.RED,
      Color.GREEN, Shader.TileMode.REPEAT);
      参数：第一个是x开始坐标，第二个是y开始坐标，三、四结束x、y坐标，左边是根据view的坐标系开始计算的，五是模式：重复还是扩展、镜像
- RadialGradient
		辐射渐变，由一中心点渐变
- SweepGradient 
		像雷达一样
- BitmapShader 
		图像BitmapShader(Bitmap bitmap, Shader.TileMode tileX, Shader.TileMode tileY)
- ComposeShader
		多个着色器组合使用
        new ComposeShader(shader1, shader2, PorterDuff.Mode.SRC_OVER);  