　　**Rigidibody**组件可为游戏对象赋予物理属性，为游戏开发者提供如在现实世界中物体受到力的作用的逼真效果（游戏引擎对其进行物理效果模拟）。

### １.使用注意事项

#### (1).父子关系

受物理系统控制，其刚体运动会随着游戏对象所属父对象的运动二运动，其依然受重力作用的影响下落。

####　(2).动画

动力学刚体会影响其他的游戏对象，但自身不受到物理系统影响？

#### (3).脚本

通过脚本为游戏对象添加作用力和扭矩力，通过刚体的**AddForce**和**AddToraue**函数。



### 2.刚体属性面板参数设置



![img](./Resources/rigidbody_ins.png)

| 选项                    | 详解                                                         |
| ----------------------- | ------------------------------------------------------------ |
| Mass                    | 质量                                                         |
| Drag                    | 空气阻力                                                     |
| **Angular Drag**        | 角阻力，游戏对象**受到扭矩力**时受到空气阻力的大小。         |
| Use Gravity             | 使用重力                                                     |
| **Is Kinematic**        | 开启动力学，开启有游戏对象只能通过<br>Transform组件对其控制，不再受到物理系统的影响 |
| **Interpolate**         | 控制刚体抖动情况:<br>None:无差值<br>Interpolate:内差值，基于上一帧Transform平滑此次Transform<br>Extrapolate:外差值，基于下一帧平滑此次Transform |
| **Collision Detection** | 用于避免高速运动对象无法与其他对象发生碰撞:<br>1.Discreate:离散碰撞检测，游戏对象与场景中其<br>   他所有碰撞体进行碰撞检测；<br>2.Continuous:连续碰撞检测，检测游戏对象与动<br>   态碰撞体的碰撞，<font color=#ff0000>使用连续碰撞检测模式来检测<br>   与网络碰撞体的碰撞；</font><br>3.Continuous Dynamic:连续动态碰撞检测。检测<br>连续碰撞模式或连续动态碰撞模式对象的检测<br>[Collision Detection检测模式](https://blog.csdn.net/Ming991301630/article/details/77922493)  [贴吧解释](http://tieba.baidu.com/p/2741418294) |
| Constraints             | 约束刚体运动：<br>Free Position(冻结x,y,z部分或全部位置)，<br>Free Rotation(冻结x,y,z部分或全部旋转) |

