## 疫情传播模拟系统

- 目录
  - 背景介绍
  - 技术栈
  - 代码运行
  - 建模思路
  - 实验过程
  - 结果分析
  - 防控建议

### 背景介绍

​     为研究新冠病毒在人群中的传播情况，讨论各种防疫措施（佩戴口罩，限制出行，增加医院以及救治速度）对疫情传播的影响。从个人、政府、医疗的三个方面讨论疫情的传播。

### 技术栈

* 前端

     采用了简单的html+javascript+boostrap编写前端页面和小动画。

* 后端

  ​    结合了python的flask web框架进行前后端数据的交互，前端模拟完成返回json数据，利用python的document库以及绘图库绘制了简单的报表。

### 代码运行

* 项目录下运行cmd
* pip install -r requirements.txt安装python依赖
* 运行python app.py  

### 建模思路

* 具体规则

    单个小球模拟为一个人，小球直径为30px长度，当两人距离为两个身位60px或者发生碰撞时，此时可能发生感染事件。其中，蓝色代表潜伏期，红色代表已经感染，棕色不移动代表死亡，黄色代表此时已康复拥有抗体，黑色代表健康未感染。

* 参数设置

  ​      未戴口罩且与潜伏期的人距离过近或发生碰撞时：有抗体1%感染几率，无抗体5%感染几率。

  ​      未戴口罩且与感染期的人距离过近或发生碰撞时：有抗体5%感染几率，无抗体30%感染几率。

  ​      戴口罩且与潜伏期的人距离过近或发生碰撞时：有抗体0.3%感染几率，无抗体1%感染几率。

  ​      戴口罩且与感染期的人距离过近或发生碰撞时：有抗体3%感染几率，无抗体15感染几率。

  ​      人群可能发生聚集，当小球之间长时间距离过短时候，发生感染的概率自然会大幅度上升。人群移动为随机移动，一共设置为5档。医院开启之后，设置收容速度为3档（慢中快），每次收容数量为5人，医院容纳数量可自主设置。

* 模型建立

     根据上述规则，用JavaScript进行程序编写，并实现了数据的可视化。

     综合考虑程序效率、运行时间，我们的网格采用813*830（单位：px）。

     运行之后，会返回一份报表，我们主要观察分析感染人数、潜伏人数，死亡人数随着时间的变化。

​    

  * 主要代码

    * 小球随机移动函数，小球碰到边界后改变移动方向，确保在框内移动。移动的单位为像素点，可以自主设置速度，一共5档。

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/50d77bbe0fb79fb441f1387fc03b64a.png)

    * 设置感染后潜伏日期为14天，若处于潜伏期且没有到医院进行救治，则小球移动14天后进入感染期，颜色变为红色。

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303175452.png)

    * 假设感染的人不救医导致死亡的概率为0.1%，小球变为棕色表示死亡；不救医而痊愈的概率为0.3%，小球变为黄色表示痊愈；

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303175817.png)

    * 小球碰撞函数，遍历除自己之外的所有小球的位置坐标，判断小之间的位置关系。若发生碰撞，根据当前小球与被撞小球的相对方向，改变当前小球的移动方向

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303181037.png)

    * 传染函数，若发生碰撞或距离太近时，根据不同情况（是否带口罩，是否有抗体，处于潜伏期还是感染期）设置不同的感染几率，若小球进入潜伏期，则变为蓝色。

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303182811.png)

    * 医院收纳函数，医院收纳速度共有3档，可以前端自行设置挡位，默认为2档（中等速度）。在1档（最慢速度）下，每3秒收纳5个病人；在2档下，每2秒收纳5个病人；在3档（最快速度）下，每1秒收纳5个病人。

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303184004.png)

    * 医院救治函数，医院救治速度共有3档，可以前端自行设置挡位，默认为2档（中等速度）。假设病人被治愈的几率为20%，治愈后小球变为黄色，代表此时已康复拥有抗体。

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303184800.png)

    * 对前端输入的数据（医院容量，初始感染人数，人群移动速度等）做出规定，如果输入的数字超出规定最大值，纠正或提示。

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303185209.png)

    ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303185218.png)

    

### 实验过程以及可视化展示

对于 每种情况，我们设置了2~3次实验。

* 是否佩戴口罩

  在其他参数不变的情况下，观察是否佩戴口罩对病情传播的影响，参数设置如下：

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%9B%BE%E7%89%871.png)

  * 佩戴口罩时，感染率为16.7%，死亡率为0%

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303203631.png)

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303202205.png)

     可视化展示：

  <video id="video" controls="" preload="none">
      <source id="mp4" src="https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E6%9C%89%E5%8F%A3%E7%BD%A9.mp4" type="video/mp4">
  </video>


  * 不佩戴口罩，感染率为34.9%，死亡率为4.3%。

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303204024.png)

    可视化展示：

  <video id="video" controls="" preload="none">
      <source id="mp4" src="https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E6%97%A0%E5%8F%A3%E7%BD%A9.mp4" type="video/mp4">
  </video>

* 限制出行

   在其他参数不变的情况下，观察是否限制出行对病情传播的影响，参数设置如下：

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303210146.png)

  * 设置人员移动速度为5档（最快），开启人员聚集，感染率为46.1%%，死亡率为18.5%。

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303205955.png)

    可视化截图（小球为棕绿色代表死亡，小球为黄色代表感染后康复）：

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303210001.png)

  * 设置人员移动速度为1档（最慢），关闭人员聚集，感染率为15.4%，死亡人数为17，死亡率为6%。

​      ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303211109.png)

​         模拟过程截图：（黑色小球表示健康人，蓝色小球表示处于潜伏期）

![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303211402.png)

* 医院收纳和救治速度

  在其他参数不变的情况下（假设不限制出行，开启聚集，不佩戴口罩，移动速度为5档），观察是医院医院收纳和救治速度对病情传播的影响。

  * 当医院收纳速度和救治速度都为3档（最快速度）时，死亡人数为7，死亡率为6%，感染人数为41，感染率为27.3%。

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303214035.png)

  

  * 当医院收纳速度和救治速度都为1档（最慢速度）医院容纳量为50时，死亡人数为28，死亡率为20.3%，感染人数为84，感染率为43.3%。

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303213404.png)

​      可视化模拟过程展示：

<video id="video" controls="" preload="none">
    <source id="mp4" src="https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%8C%BB%E9%99%A2%E4%B8%8D%E8%A1%8C.mp4" type="video/mp4">
</video>


​         

* 综合

  * 当医院救治速度设置为最快，容量设置为最大，并且要求佩戴口罩，限制出行时，死亡人数为2，死亡率仅为1.9%%，感染人数为3，感染率为2.8%。

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303214617.png)

  可视化模拟过程截图：（黑色小球代表健康的人）

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303214827.png)

  * 当医院救治速度设置为最慢，容量很小，并且不佩戴口罩，不限制出行的情况下，死亡率高达22.5%，感染率高达41.8%。

  ![](https://github.com/Shawtykk/NWUzhangyukun/blob/main/pic/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210303215853.png)

### 结果分析

* 佩戴口罩对疫情传播的影响

​       由上述结果来看，新冠疫情当中，人们通过戴口罩降低传染概率这一举措是非常有效的。在医院救治较慢和不限制出行的条件下 ，佩戴口罩时，感染率约为16.7%，不佩戴口罩，感染率可达34.9%，死亡率可达4.3%，死亡率和感染率明显上升，感染率为不佩戴口罩时的一倍左右。

* 限制出行对疫情传播的影响

​      由上述结果可知，限制出行是三种方法中最有效的控制疫情传播的方法， 如果不降低人群流动的速度，允许人群聚集，大大增大了病毒传播的机会，感染率可高达到46.1%，死亡率也可高达为18.5%。而在限制出行和禁止人群聚集的条件下，感染率为15.4%，死亡率为6%。在可视化模拟过程中也可以直观地看到两种情况的差别，前者在规定区域内存在大面积的绿色小球（表示死亡），后者在规定区域内多数为黑色小球（表示健康的人），说明在防控疫情的任何阶段，都要特别注意人群的流动，如果不对出行进行限制，疫情很可能会在短期时间内快速传播，造成严重后果。

* 医院收纳速度，救治速度，容量对疫情传播的影响

​      医院收纳速度，救治速度，容量对于疫情传播也有很大的影响，医院收纳救治速度很低，容量较小，死亡率可达20.3%，感染率可高达43.3%，在医院收纳救治速度很快且容量充足的情况下，死亡率1.9%左右，感染率为2.8%左右。可以看出，医院的防疫水平在防疫中起着重要作用，如果做到有病快速就医，医院设备先进，治疗水平较高，那么可以大幅度降低感染率和死亡率。

* 综合

    积极的采取各种防疫措施，例如佩戴口罩，减少出行，避免人群聚集，及时就医可以显著的降低疫情传播速度，死亡率可低至1.9%，感染率低至2.8%。若不采取措施，医疗技术和设备落后的情况下，死亡率可高达22.5%，感染率高达41.8%。

     

### 防控建议

​     结果证明，戴口罩、少外出这些看似简单的举措，其实会很好地控制住疫情的进一步传播，少外出对于控制疫情传播有非常明显的帮助，医疗投入对于疫情的控制也起到至关重要的作用。上述分析得出：当三种参数同时控制的很好时，疫情防控会取得更好的结果，也就是人民、政府、医护三方面齐心协力才可共度难关。



