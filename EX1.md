[![pA1ssaT.png](https://s21.ax1x.com/2024/09/29/pA1ssaT.png)](https://imgse.com/i/pA1ssaT)
# <center>2024秋季学期</center>
# <center>《开源地理信息系统》技术报告</center>
## <center>题目一：基于开源GIS软件水体自动遥感提取及人工改正</center>
### <font face="仿宋"><center>姓 名:  <u>袁婧</u></font>
### <font face="仿宋"><center>学 号：<u>20221170067</u></font>
### <font face="仿宋"><center>院 系：<u>地球科学学院</u></font>
### <font face="仿宋"><center>专 业：<u>地理信息科学</u></font>
### <font face="仿宋"><center>任课老师：<u>罗新老师</u></font>
### <font face="仿宋"><center>二〇二四年九月</font>


### <font color=orange>一、实验数据：</font>
1. **原始数据：**
- 哨兵2号影像：l7_scene_21.tif
- 水体提取矢量结果：l7_scene_21_pred.gpkg
2. **结果数据：**
- 基于开源软件与人工修正的水体矢量数据：l7_scene_21_pred_new.gpkg
- 水体提取结果出图
- 对比精度结果
### <font color=orange>二、实验目的：</font>
- 利用开源桌面GIS软件采用水体指数方法进行水体信息遥感提取。
- 对所提供水体提取结果进行人工改正，获得准确水体范围。
- 对比水体指数方法获得水体范围和所提供水体提取结果精度情况。
### <font color=orange>三、实验步骤：</font>
1. **计算mndwi：**

基于改进的归一化水体指数MNDWI提取水体，其公式为：
$$
MNDWI=\frac{B_{green}-B_{SWIR}}{B_{green}+B_{SWIR}}
$$
对于本实验的6波段sentinel数据，其公式为：
$$
\frac{band\_2-band\_5}{band\_2+band\_5}
$$
基于开源软件snap计算结果为-0.68至1.15，但-1<=mndwi<=1，存在大于1的异常值，因此通过```if mndwi>1 then 1 else mndwi```消除了大于1的异常值，最终得到mndwi结果为[-0.68,1.00]，结果如下图：  

[![pA10mX6.png](https://s21.ax1x.com/2024/09/29/pA10mX6.png)](https://imgse.com/i/pA10mX6)

2. **mndwi指数值影像二值化：**

通过snap得到mndwi的影像直方图如下所示:  

[![pA103hd.png](https://s21.ax1x.com/2024/09/29/pA103hd.png)](https://imgse.com/i/pA103hd)

基于该直方图，最初选择0.2作为阈值，基于语句：```if mndwi>0.2 then 1 else 0```得到二值化结果如下图所示：

[![pA10G9A.png](https://s21.ax1x.com/2024/09/29/pA10G9A.png)](https://imgse.com/i/pA10G9A)  

观察发现：白色部分不仅包括了水体，也包括了冰川，粗略地采用0.2作为阈值是不准确的。阅读论文《基于NDWI-NDSI组合阈值法的布加岗日冰湖提取及其变化分析》[遥感学报]，推测：
- 虽然根据图2发现整幅图像的mndwi基本为二峰，但是由于图像较大，可能在局部地区的NDWI的直方图实际呈现三峰，即以0.2为阈值以上的范围内不仅包含了水体，也包含了冰川；  

- 对比0.2为阈值的MNDWI提取的水体与RGB影像目视解译对比可知，提取结果出现非水体区域。
依据语法：```if (mndwi>0.2) and (mndwi<0.75) then 1 else 0```对比了0.2到0.75，0.2到0.8，0.2到0.85，0.2到0.9等不同阈值下水体的提取结果如下图所示:  

[![pA10dHS.png](https://s21.ax1x.com/2024/09/29/pA10dHS.png)](https://imgse.com/i/pA10dHS)  

最终参考OTSU（大律法）与目视解译，选择了0.2、0.88作为阈值。在人工精分类过程中，发现初步分类结果中，存在大量如图所示红圈中的分类错误，误将积雪、冰舌分类为水体。通过人工分类将其删除。  

[![pA10Dhj.png](https://s21.ax1x.com/2024/09/29/pA10Dhj.png)](https://imgse.com/i/pA10Dhj)
[![pA10y3n.png](https://s21.ax1x.com/2024/09/29/pA10y3n.png)](https://imgse.com/i/pA10y3n)


### <font color=orange>四、实验成果与分析：</font>

1.**成果**  
实验成果如图所示，左图为所提供水体提取结果，右图为Mndwi与目视解译获得水体范围。  

[![pA1sQKI.png](https://s21.ax1x.com/2024/09/29/pA1sQKI.png)](https://imgse.com/i/pA1sQKI)
  
2.**精度评价**  
使用ENVI混淆矩阵工具评价分类精度。在进行之前，需要进行以下预处理：  
1）将分类完成的.gpkg(.shp)文件通过QGIS转换为栅格文件。此时数据存在nodata，还需要进一步处理。  
2）通过```Con(IsNull("water"),1,"water")```为nodata的像素赋0值。  
3）基于envi工具，进行彩色密度分割，此时slices包括了水体与非水体。  
4）将tif格式的密度分割结果转换为ENVI分类类型图像。（ENVI中的混淆矩阵验证工具只接受ENVI格式）  
5）画取训练样本，进行混淆矩阵精度评价。

以下为所提供水体提取精度报告（前）与MNDWI水体提取精度报告(后)。  

[![pA1s2RJ.png](https://s21.ax1x.com/2024/09/29/pA1s2RJ.png)](https://imgse.com/i/pA1s2RJ)[![pA1sciF.png](https://s21.ax1x.com/2024/09/29/pA1sciF.png)](https://imgse.com/i/pA1sciF)  

|方法|总体精度|Kappa|
|:--:|:--:|:--:|
|<div style="width:200px">所提供水体提取结果方法|<div style="width:200px">99.9726%|<div style="width:200px">0.9993|
|MNDWI|94.6287%|0.8473|
  
  
- 不足之处1：训练样本是根据目视解译建立，水体和非水体各画50个，均匀分布在影像里。由于水平不足，样本的选取存在偏差，可能导致精度评价结果不准确。
- 不足之处2：单凭MNDWI，分类结果精度一般，需要经过大量的人工改正。或许可以通过神经网络、支持向量机等监督分类，或IsoData等非监督分类方法来修正分类结果。    

[遥感学报]:https://www.ygxb.ac.cn/zh/article/doi/10.11834/jrs.20210205/
