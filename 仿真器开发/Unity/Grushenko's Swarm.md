# Grushenko's Swarm


## 项目结构
### Project
Assets
* Models
* **Prefabs**
	- Bird
		+ 一个隐藏的 Sphere（没用，应该是导入模型前测试用的）
		+ arrow-sketchup-z，fbx 模型文件
* Resources
* Scenes

（以下是脚本）

* Bird
	- 有两组表示位置和速度的数值（Position，Velocity，alpha 和 theta 用于在 SimulationStep 中计算 Velocity），一组当前值和一组 new
	- 对象的方法为：更新实际 transform 的位置，更新当前位置和速度（用 new）
* CubeOutline
	- 渲染 \#TODO
* FreeCamera
	- 功能：WASD 或方向键移动，QE 上下移动， RF 和 PgUp PgDown 让世界上下移动，Shift 快速移动，右键和鼠标调整视角
	- 问题：移动视角之后，怎么让方向的移动是相对局部坐标而不是世界坐标
		+ 回答：transform.right 之类的向量考虑旋转，世界坐标用的是 Vector3.right 等
* SettableText
	- 功能：获取一个 float/double 值，将脚本所挂载对象的 Text 设为该值
* Spawner
	- Update, FixedUpdate
		+ （注：两者都会执行）Update 里更新 Bird 的实际信息，FixedUpdate 里进行仿真模拟
	- OnDrawGizmos
		+ Gizmos 是 Unity 的调试工具，OnDrawGizmos 每一帧都会执行，其中绘制的内容只能在 Scene 视图看到。例如下面的白线和红圈：
			![image-20221028103429758](E:/Note/Img/image-20221028103429758.png)
	- SimulationStep(double deltaTime)
		+ 模拟一步，时间长度为 deltaTime
* Vec3D
	- 自己实现的三维向量
	- 方法包括：点乘（Times），加法（Add），单位化（Normalize），距离、长度等（很简单）。

### Hierarchy
* Main Camera
	- Camera
		+ Clear Flags 设为Solid Color，纯黑色
	- 【#】Free Camera
* Directional Light
* Canvas
	- Button 和 Slider 控件在左上角，Text 在左下角
	- On Click 都在 Spawner 脚本中
		+ InitButton：FixedInit()
		+ StepButton：FixedSimulationStep()
		+ StartButton：StartSimulation()
		+ StopButton：StopSimulation()
	- On Value Changed 部分也在 Spawner 脚本中，还有用于修改文本内容的在 Settable Text 中（注意挂载的首先是一个游戏物体，然后下拉列表选择游戏物体的组件，最后是组件中的方法）
		+ SimulationSpeedSlider：SetSimulationSpeed()
		+ RadiusSpeedSlider：SetRadiusSpeed()
		+ BirdsNumberSpeedSlider：SetNumberOfParticle()
		+ NoiseIntensitySlider：SetNoiseIntensity()
		+ 每个都包含 SetNumericText()，在 Settable Text 中
* EventSystem
* Spawner
* CubeOutline
	- 【#】Cube Outline
