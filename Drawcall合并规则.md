# Drawcall合并规则
### 层级号计算规则
1. 如果有一个UI元素，它所占的屏幕范围内（通常是矩形），如果没有任何UI在它的底下，那么它的层级号就是0（最底下）；
2. 如果有一个UI在其底下且该UI可以和它Batch，那它的层级号与底下的UI层级一样；
3. 如果有一个UI在其底下但是无法与它Batch，那它的层级号为底下的UI的层级+1；
4. 如果有多个UI都在其下面，那么按前两种方式遍历计算所有的层级号，其中最大的那个作为自己的层级号。
### 合并批次
1. 同层级可以Batch就会进行Batch 
2. 相邻层级交界处如果可以Batch就会进行Batch（相同材质，相同Mesh网格）

> 同层级Image优先Text渲染（Unity2017.4.33测试结果）

```
修改Pos_z ,Rot_x,Rot_y 会打断批次，前后分开进行自己的层级计算
```
# 样例解析
### 先按照层叠顺序处理，图A处理后和图B相同
![A.png](https://github.com/Cairneqi/UGUI-Optimize/blob/master/A.png)
![B.png](https://github.com/Cairneqi/UGUI-Optimize/blob/master/B.png)
### 然后计算层级号如图B所示，按层级号依次渲染
1. Drawcall-1 ：0层级（I-B，I-G，I-I）(测试是Image先渲染)
2. Drawcall-2 ：0层级（T-A，T-D）
3. Drawcall-3 ：1层级（I-E） （上一层级网格不同不能Batch）
4. Drawcall-4 ：1层级（T-C，T-H，T-J）
5. Drawcall-5 ：2层级（I-K）
6. Drawcall-6 ：2层级（T-F）
