## 在Unity中实现相机抖动特效

原创： Unity [Unity官方平台](javascript:void(0);) *今天*

你是否想要为游戏加入更强的真实感？你是否想要为游戏添加一些屏幕抖动特效？

为游戏加入真实感最好的一个方法就是使用相机抖动特效。这个方法非常适用于突显动作效果，使相机的感觉更为真实，并让你感觉自己成为了游戏的一部分。本篇文章将帮助你了解通过三种方法创建相机抖动特效。

![img](/Resources/001_相机抖动.gif)

实现相机抖动特效的三种方法

本文提供完整的工程示例下载，请访问：

http://github.com/tejas123/different-ways-of-shaking-camera-In-unity/archive/master.zip

#### 1.通过脚本

首先在Unity设置如下图的场景。

![img](/Resources/001_scence.webp)

创建一个名为Explodestar的空白游戏对象，在其子对象部分创建粒子系统，让其实现更明显的效果。创建名为Explodestar的脚本，附加到Explodestar游戏对象上，这样能在游戏运行时播放粒子系统。

创建另一个名为CameraShake的脚本，将其附加到将要加入抖动特效的摄像机上。CameraShake.cs代码如下所示。

```C#
using UnityEngine;
using System.Collections;

public class CameraShake : MonoBehaviour
{
   public IEnumerator Shake(float duration, float magnitude)
    {
        Vector3 orignalPosition = transform.position;
        float elapsed = 0f;

        while (elapsed < duration)
        {
            float x = Random.Range(-1f, 1f) * magnitude;
            float y = Random.Range(-1f, 1f) * magnitude;
            transform.position = new Vector3(x, y, -10f);
            elapsed += Time.deltaTime;
            yield return 0;
        }
        transform.position = orignalPosition;
    }
}
```

CameraShake脚本中，协程将用于实现相机抖动效果。

ExplodeStar.cs脚本代码如下所示。    

```c#
using UnityEngine;
using System.Collections;

public class ExplodeStar : MonoBehaviour
{
    public ParticleSystem explodePartical;
    public CameraShake cameraShake;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.E))
        {
            explodePartical.Play();
            StartCoroutine(cameraShake.Shake(0.15f, 0.4f));
        }
    }
}
```


现在返回到Unity中，点击Play按钮，然后按下E键。屏幕就开始抖动了。这是一个实现抖动相机非常简单快捷的方法。

但是该方法不会以任何方式旋转摄像机，而且也无法控制粗糙度。该方法只会在每帧移动摄像机，而且使用协程有时会略显笨重。如果你想要更平滑的相机抖动特效，不推荐使用该方法。

#### 2.使用动画

首先在Unity设置如下图的场景。

![img](/Resources/001_scence2.webp)

为主摄像机添加Animator，在该Animator中创建二个动画，分别是闲置动画和抖动动画。

在闲置动画中，开始进行录制，并设置摄像机到其原始位置。在抖动动画中，每二帧移动摄像机位置，实现抖动效果。

 

现在进入Animator中创建抖动特效触发器。在闲置和抖动动画之间创建过渡效果，取消勾选‘has exit time’，设置过渡时间为1。

![img](/Resources/001_animator.webp)

然后制作抖动动画到闲置动画的过渡，设置过渡时间为1，但是不要取消勾选‘has exit time’。

![img](/Resources/001_animator2.webp)

创建名为ShakeWithAnimation的空对象，添加ShakeWithAnimation脚本到该对象上，在子对象部分使用粒子系统，使效果更明显。

ShakeWithAnimation代码如下所示。

```C#
using UnityEngine;
using System.Collections;

public class ShakeWithAnim : MonoBehaviour
{
   public Animator camAnim;
   public ParticleSystem explodeStar;

   void Update()
   {
       if (Input.GetKeyDown(KeyCode.A))
       {
           camAnim.SetTrigger("Shake");
           explodeStar.Play();
       }
   }
}
```



现在返回到Unity中，点击Play按钮,然后按下A键。屏幕又开始了晃动，这是抖动相机的另一种方法。

 

虽然所有条件都满足了，但是粗糙度怎么办？有没有什么方法可以使用动画来控制粗糙度吗？答案是否定的。那么我们该怎么解决这个问题呢？让我们尝试通过插件来解决这个问题。

#### 3.使用插件

EZ Camera Shake是在Asset Store资源商店广受好评的五星资源，它可以实现相机抖动功能，用来创造许多效果，例如地震爆炸所带来的抖动。

 

首先访问Asset Store资源商店，下载EZ Camera Shake插件，下载地址：

https://www.assetstore.unity3d.com/cn/?stay#!/content/33148

 

![img](/Resources/001_EzShake.webp)

在EZ Camera Shake下载完成后，将其导入到Unity新建场景中。场景将新增EZCameraShake文件夹。添加EZCameraShake的CameraShaker脚本到主摄像机上，根据需要设置变量。

 

现在创建空白对象，命名为ExplodeHeart，在子对象部分加入粒子系统，让效果更明显。创建并加入ExplodeHeart脚本到该粒子系统上。为脚本添加EZCameraShake命名空间。

ExplodeHeart脚本代码如下所示：

```C#
using UnityEngine;
using System.Collections;
using EZCameraShake;

public class ExplodeHeart : MonoBehaviour
{
    public ParticleSystem explodePartical;
    
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.H))
        {
            explodePartical.Play();
            CameraShaker.Instance.ShakeOnce(4f, 4f, 0.15f, 1f);
        }
    }
}
```



现在进入Unity中播放场景，按下H键。屏幕抖动效果会比之前二种方法更平滑。这样就通过使用插件来实现了相机抖动效果。

这样是否意味着通过脚本和动画来抖动相机就不好呢？当然不是这样，最后一种方法会在运行时给内存增加更多负担，如果你不需要这么平滑且控制好粗糙度的的抖动效果，那么使用简单快捷的脚本方法就够了。所以必须按照要求，聪明地选择游戏中要使用的相机抖动方法。

#### *小结

三种实现相机抖动特效的方法你掌握了吗？一定要针对项目中的实际情况选择合适的方法。更多Unity的技术教程尽在Unity官方中文论坛(UnityChina.cn)！