[![pA1ssaT.png](https://s21.ax1x.com/2024/09/29/pA1ssaT.png)](https://imgse.com/i/pA1ssaT)
# <center>2024秋季学期</center>
# <center>《开源地理信息系统》技术报告</center>
## <center>题目二：开源项目运行环境配置及使用</center>
### <font face="仿宋"><center>姓 名:  <u>袁婧</u></font>
### <font face="仿宋"><center>学 号：<u>20221170067</u></font>
### <font face="仿宋"><center>院 系：<u>地球科学学院</u></font>
### <font face="仿宋"><center>专 业：<u>地理信息科学</u></font>
### <font face="仿宋"><center>任课老师：<u>罗新老师</u></font>
### <font face="仿宋"><center>二〇二四年十月</font>


### <font color=orange>一、实验数据：</font>
1. **原始数据：**
- 内江市市区哨兵2号影像：.tif
2. **结果数据：**
- 基于开源项目代码的内江市市区水体提取结果：.tif；
- 内江市市区水体专题地图。
### <font color=orange>二、实验目的：</font>
- 配置开源项目:https://github.com/xinluo2018/WatNet运行环境；
- 根据开源项目使用说明，利用开源项目代码提取家乡所在地水体信息；
- 制作家乡所在地水体专题地图。

### <font color=orange>三、实验步骤：</font>
1. **下载github源代码**  
`git clone https://github.com/xinluo2018/WatNet.git`

2. **配置编程环境**  
阅读WatNet文件下代码，需要下载tensorflow, gdal, numpy, matplotlib, sklearn, pandas等。  

- 如果无法通过anaconda安装特定版本的库，可以通过.whl文件，在终端通过pip安装，例如：  [![pAaC8Lq.png](https://s21.ax1x.com/2024/10/19/pAaC8Lq.png)](https://imgse.com/i/pAaC8Lq)
通过https://github.com/fo40225/tensorflow-windows-wheel下载对应版本的.whl文件到本地文件夹，再在该文件夹中通过`pip install tensorflow-2.5.0-cp39-cp39-win_amd64.whl`完成安装。  

3. **获取Sentinel-2遥感影像与处理**  
- 通过欧空局查找并下载内江市sentinel-2遥感图像  
[![pAaAjLd.png](https://s21.ax1x.com/2024/10/19/pAaAjLd.png)](https://imgse.com/i/pAaAjLd)
- 通过snap，适当裁剪需要的研究区域，并进行波段选取：  
[![pAain2Q.png](https://s21.ax1x.com/2024/10/19/pAain2Q.png)](https://imgse.com/i/pAain2Q)
也可以通过python代码进行裁剪。(x_min_subs_update, y_max_subs_update)  
  
  [![pAaCTmt.png](https://s21.ax1x.com/2024/10/19/pAaCTmt.png)](https://imgse.com/i/pAaCTmt) 选取出4个10米分辨率波段和2个20m中红外波段，叠加成一个.tif文件。需要注意叠加文件的次序，波段依次为：蓝，绿，红，近红外，中红外-1， 中红外-2，即波段2, 3, 4, 8, 11, 12。 
- 重采样  
基于Snap软件，Raster-Geometric-Resamping，完成分辨率10m，Bilinear重采样，输出.tif格式的内江市市区影像，并添加到data/test-demo目录下。  

4. **基于源代码，修改文件名与参数，提取出影像中水体信息**  
其中参数为存放遥感影像路径，水体提取结果自动保存于相同目录。
### <font color=orange>四、实验成果与分析：</font>

1.**基于代码的水体提取成果**  
- Load and prepare the satellite image data代码块的运行成果如图所示，读取了原始影像：
[![pAaESot.png](https://s21.ax1x.com/2024/10/19/pAaESot.png)](https://imgse.com/i/pAaESot)
- show the result代码块的运行成果如图所示，左图为调整了颜色通道顺序和亮度的图像，右图为WatNet水域分布提取图像：  
  [![pAaE9FP.png](https://s21.ax1x.com/2024/10/19/pAaE9FP.png)](https://imgse.com/i/pAaE9FP)
- img_info代码块的运行成果如图所示，包含了地理范围，角点坐标，分辨率，旋转角度，行列数，波段等：
  [![pAaEPW8.png](https://s21.ax1x.com/2024/10/19/pAaEPW8.png)](https://imgse.com/i/pAaEPW8)  

2.**内江市市区水体专题地图**  
- WatNet提取水体结果如图所示。可以观察到清晰观察到东兴区的沱江，包括了横跨沱江的沱江大桥、西林大桥、内江新坝大桥（在水体上呈现为黑色细线），以及距内江市城区14公里，位于永安镇圆坝村境内，属沱江水系乌龙河支流的黄鹤湖（图像左下），水体提取结果较清晰。
[![pAaVpnJ.md.png](https://s21.ax1x.com/2024/10/20/pAaVpnJ.md.png)](https://imgse.com/i/pAaVpnJ)  
[![pAaeRt1.png](https://s21.ax1x.com/2024/10/20/pAaeRt1.png)](https://imgse.com/i/pAaeRt1)
- 另外，基于MNDWI对同一研究区域进行水体提取：  
$$
MNDWI=\frac{B_{green}-B_{SWIR}}{B_{green}+B_{SWIR}}
$$  
其结果与基于WatNet提取结果的对比如下所示：  
[![pAaEzX4.png](https://s21.ax1x.com/2024/10/20/pAaEzX4.png)](https://imgse.com/i/pAaEzX4)

### 观察发现，相较MNDWI：  
**边界清晰度：**  
以左下角黄鹤湖为例，深度学习提取的水体轮廓更加清晰，边界更加准确。  
**识别能力：**  
整个研究区域内，深度学习对面积较小水体的提取更加准确。  
综上，WetNet提取成果精度优于MNDWI。


### <font color=orange>五、总结与反思：</font>  
1.**问题1与解决方法**  
使用python虚拟环境，发现一直显示导入包有点问题，发现在虚拟环境的python路径里，电脑装的Python的路径顺序竟然排在虚拟环境之上。  
且别无法通过修改PYTHONPATH解决该问题。尝试在Lib\site-pakages下新建sitecustomize.py并运行代码：  
[![pAaPo34.png](https://s21.ax1x.com/2024/10/19/pAaPo34.png)](https://imgse.com/i/pAaPo34)
完成删除后，再查看sys.path已恢复正常，如下图所示：
  [![pAaPB4S.png](https://s21.ax1x.com/2024/10/19/pAaPB4S.png)](https://imgse.com/i/pAaPB4S)
  此时能成功运行。
  
  2.**问题2与解决方法**  
- 在配置环境时，需要注意各个库的下载顺序：  
实践发现：先安装numpy，再安装tensorflow，可能存在版本冲突导致报错，查阅资料后选择安装次序；  
解决方法：***tensorflow–>matplotlib–>seaborn->sklearn(scikit-learn)***，采用以上的顺序安装就不需要安装pandas和numpy等依赖库，因为已经都在上述几个的安装过程中安装好了。  

- 注意各个库的版本是否兼容：
在查阅资料与不断安装尝试后，各个库的版本分别为：  

|库|版本|
|:--:|:--:|
|<div style="width:265px">tensorflow|<div style="width:265px">2.10.0|
|matplotlib|3.9.2|
|scikit-learn|1.5.1|
|pandas|2.2.2|