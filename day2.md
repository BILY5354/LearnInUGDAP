# [第二天](./day2.md)
1. [上午](#1)
2. [下午](#2)
3. [](#3)
4. [](#4)  
## 1

_上午_，制作一个会移动的小球。

### 1.1 开发环境介绍：



需要安装的开发工具有：Visual Studio22 + Unity2018 + Unity Hub(用于管理开发项目，一部电脑可以有不同的Unity版本)。

- 设置Unity与VS的关联: 选择Edit->Prreferences->点击External Script Editor选择文件目录(也就是VS的安装目录)->按图中位置选择devenv.exe

![](img_day2/1_1.PNG)

- 界面调整(使便于开发查看)：序号1到4分别为Scene开发者窗口、游戏玩家视角、项目组成目录以及控制台输出用于查看脚本本件的error

![](img_day2/1_2.PNG)

- 为不同类型文件创建文件夹便于管理：C#Script用于保存本项目所有的脚本文件、Materials用于保存所有材质球(下午用，用于给物体设置材质)、prefeb用于保存会多次使用的物体(下午用)以及Scence用于所有场景保存(今天的项目只有一个场景)。

![](img_day2/1_3.PNG)

### 1.2 场景配置，放置小球和场地

- 放置地板并设置大小，这次项目是3D的，所以选择3D Object的Plane。但是Plane这个名字不好知道是什么意思，所以将其改名为Ground。并在Inspector中调整相应的参数。

<img src="img_day2/1_4.PNG" style="zoom:80%;" />

<img src="img_day2/1_4_1.PNG" style="zoom:80%;" />

- 放置小球及基础设置，小球也是选择3D的并重命名为Player(毕竟是玩家要操作的对象嘛)。然后发现小球只露出了半个球，转动下镜头发现还有半个在下面！这时候将Position的Y轴设为0.5即可，**因为Unity是以物体中心为坐标轴，然后小球的直径为1**。

<img src="img_day2/1_5.PNG" style="zoom:80%;" />

<img src="img_day2/1_5_1.PNG" style="zoom:80%;" />

- 小球的重力设置：有了小球后，可以在Inspector中添加Rigidbody，即刚体属性用于小球的运动。**一定要确保是在这个球体的Inspector页面中点击Add Component**搜索Rigidbody。不要选择有后缀2D**因为本次项目是3D的**。确保勾选上重力就好了。

<img src="img_day2/1_6.PNG" style="zoom:80%;" />

<img src="img_day2/1_6_1.PNG" style="zoom:80%;" />

- 这时候可以测试一下，把小球放到空中，看其会不会掉下去。**地板是不需要添加Rigidbody属性的**，因为我们只希望地板可以安安静静躺在空间内就好了。

### 1.3 脚本编写(写代码)

- 在Project窗口新建脚本本件并命名为PlayerController表示这是玩家控制小球的代码

<img src="img_day2/1_7.PNG" style="zoom:60%;" />

- 那么，怎么将改脚本与小球联系起来呢？把改文件直接拖入到**小球**的Inspector，。**Unity很多操作可以直接拖拽**，十分方便。

![](img_day2/1_8.PNG)

- 然后再双击该脚本本件PlayerController进入VS打代码。
- 如果一切无误的话，此时点击下图运行就可以通过wasd操作小球了。





### 1.4 上午代码

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    //Rigidbody  刚体 来实现移动
    private Rigidbody rb;
    public float speed;
    
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        Vector3 movement;

        movement.x = Input.GetAxis("Horizontal");
        movement.y = 0;//使不能跳
        movement.z = Input.GetAxis("Vertical");//前后
        rb.AddForce(movement* speed);
    }

}

```
### 可能出现问题的地方

- 在Unity或VS中没有保存，导致脚本不正确
- 

## 2

_下午_，完成小球的关卡（吃掉12个方块游戏胜利）。

### 2.1 使小球更好移动并设置墙壁防止小球掉出边界

- 早上的代码中，是怎么知道GetAxis的参数是Horizontal呢？其实不管是Horizontal或者Vertical这些是有设定的。点击File->Project Setting -> Input发现有Axes属性，里面的参数正有Horizontal和Vertical。**所以这里的属性是需要对应的**，如果将Vertical修改成MyVertical那么在代码中也要换成对应名字。

```c#
 movement.x = Input.GetAxis("Horizontal");
```

![](img_day2/2_1.PNG)

- 如果要新增一个交互键，同样也在这里修改。

- 因为使用了Rigidbody，所以小球是有惯性移动的，为了更好的移动可以在PlayerController定义一个```public float speed;```这时候再到Unity看，发现多了一个Speed属性，这样就可以让策划小伙伴帮忙调试了。

<img src="img_day2/2_2.PNG" style="zoom:80%;" />

- 这时候再移动会发现稍微好了一些



### 2.2 设置相机跟随

### 2.3 设置方块

![](img_day2/.PNG)



### 2.4 设置UI

### 2.5 程序打包

### 2.6 下午代码(总)

-  ```PlayerController.cs```小球代码

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class PlayerController : MonoBehaviour
{
    //Rigidbody  刚体 来实现移动
    private Rigidbody rb;
    public float speed;

    public GameObject countTextObj;//加载分数显示
    public GameObject winTextObj;//加载分数显示
    int count;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
        count = 0;
        Update();
        winTextObj.GetComponent<Text>().text = "";
    }

    // Update is called once per frame
    void Update()
    {
        Vector3 movement;

        movement.x = Input.GetAxis("Horizontal");
        movement.y = 0;//使不能跳
        movement.z = Input.GetAxis("Vertical");//前后
        rb.AddForce(movement* speed);
    }

    /**
     OnTriggerEnter可以很有用，
     */
    private void OnTriggerEnter(Collider other)
    {
        /*Debug.Log("Player 碰到了" +other.gameObject.name);*/
        if(other.gameObject.CompareTag("PickUp"))
        {
            other.gameObject.SetActive(false);
            count++;
            UpdateText();
            if(count >= 12)
            {
                winTextObj.GetComponent<Text>().text = "You Win";
            }
        }        
    }

    void UpdateText() 
    {
        countTextObj.GetComponent<Text>().text = "Count:" + count.ToString();
    }
}

```
- ```CameraController.cs```摄像机代码

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{

    //相机跟随：Player每次移动，相机会跟着移动。跟着？？P移动多少距离，相机也移动相应距离
    public GameObject player;

    Vector3 offset;//三维向量

    // Start is called before the first frame update
    void Start()
    {
        offset = this.transform.position - player.transform.position;
    }

    // Update is called once per frame
    void Update()
    {
        transform.position = player.transform.position + offset;
    }
}

```



## 3

```
```
## 4
```
```