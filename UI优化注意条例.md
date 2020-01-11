# UI优化注意条例
### 1.不要使用Mask组件做遮挡,矩形mask使用RectMask2D,圆形和多边形Mask，重写Image网格生成
* 使用mask会额外消耗一个Drawcall创建mask做像素剔除，并且会将Mask下的子UI与其他UI分开，两者只能各自进行层级合并
