# AICar（1）

@[2015202004|2015202008|2015202031|2015202037]

**马克飞象**是一款专为印象笔记（Evernote）打造的Markdown编辑器，通过精心的设计与技术实现，配合印象笔记强大的存储和同步功能，带来前所未有的书写体验。特点概述：
 
- **功能丰富** ：支持高亮代码块、*LaTeX* 公式、流程图，本地图片以及附件上传，甚至截图粘贴，工作学习好帮手；
- **得心应手** ：简洁高效的编辑器，提供[桌面客户端][1]以及[离线Chrome App][2]，支持移动端 Web；
- **深度整合** ：支持选择笔记本和添加标签，支持从印象笔记跳转编辑，轻松管理。

-------------------
### 前期准备
- Arduino UNO
- BT06 Bluetooth
- HC-SR04 Ultrasonic Distance Measuring
- L293D Motor Drive Shield
- 2WD Smart Robot Car Chassis Kit
- 及其他相关器材
> 下载[Arduino](https://www.arduino.cc)，用于向arduino板导入控制程序
> 下载蓝牙串口控制APP，用于连接AICar上的BT06 Bluetooth


------------------
### 线路搭建
1.底板搭建：将三个轮子固定在底盘上，两个后轮分别与马达相连，注意：马达的突起方向朝外，便于马达连接电池。
2.驱动板与arduino板上下重叠连接，置于底盘上方中部。
3.驱动板两端M1、M4四个端口与马达相连，两个马达与航模电池串联。
4.超声波测距传感器置于面包板上，面包板固定于底板前端。
5.其中蓝牙板的四个接口分别连接驱动板上的5v、Gnd、1、0四个端口。
6.超声波测距传感器的四个接口分别与驱动板左上角的SER1、SERV0_2连接。
7.Arduino板的借口可连接电脑或充电宝。


------------------
###初步启动
在未导入程序之前，AI小车可以凭借自身电路启动并行走。小车在搭建完成之后，我们尝试了第一次启动。

[初次启动的视频](http://www.example.com)

小车车轮由两块航模电车驱动，在电路通路时，小车向前行驶。由于物理器件的原因，小车左轮转速高于右轮，这就使得小车不断顺时针旋转。我们推测是由于马达驱动不均衡所造成的。解决的办法有两个：一是使用代码来人工调整小车左右轮的转速，使之平衡；二是换马达，使两个马达动力尽量均衡。

------------------
###程序导入
```java
@luckh2
//以下为本实验中不同输入所对应的功能：其中输入'>'与'<'可以人为调整两个轮胎的转速，使得小车能够直线运行
void driver() {
  if ((getstr == '<')&&(RLRatio>0.1)) {
    RLRatio -= 0.1;
    Serial.println(RLRatio);
  }  
  if (getstr == '>') {
    RLRatio += 0.1;
    Serial.println(RLRatio);
  }  
  if (getstr == '5') {
    Serial.println("stopcar");
    stopcar();
    control=0;
  }
  if (getstr == '1') {
    Serial.println("forward");
    forward();
    control=0;
  }
if (getstr == '2') {
    Serial.println("backward");
    backward();
    control=0;
  }
  if (getstr == '3') {
    Serial.println("right");
    right();
    control=0;
  }
  if (getstr == '4') {
    Serial.println("left");
    left();
    control=0;
  }
  if (getstr=='6'){
    control=1;
    Serial.println("Auto Control On");
  }
  if(getstr=='7'){
    control=0;
    Serial.println("Auto Control Off");
  }
 
```
