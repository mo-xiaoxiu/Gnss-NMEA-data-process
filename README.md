# Gnss-NMEAParser数据处理使用指南


<div align="center">
    <img src="https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/330x330.png" >
</div>







## 目的

通过输入定位有问题的时间区间的nmea原始语句，解析出经纬度等定位信息，导出可供打点的数据格式，进行数据打点来观测问题时间点是否有定位偏移等情况

<br>

## 使用

需要一份原始nmea语句，文件命名要求为前缀为`nmea.log`，打印格式如下：

```
time=1730953023
$GPTXT,01,01,01,ANTENNA OPEN*25
$GNGST,041703.000,37.8,,,,2.8,2.5,5.8*6A
$GNGGA,041704.000,2935.21718,N,10631.58906,E,1,25,0.7,299.8,M,-38.7,M,,*68
$GNGLL,2935.21718,N,10631.58906,E,041704.000,A,A*46
$GPGSA,A,3,10,12,25,28,31,32,194,,,,,,1.3,0.7,1.1*07
$BDGSA,A,3,02,05,06,09,13,14,16,23,24,25,33,38,1.3,0.7,1.1*25
$GLGSA,A,3,78,79,69,68,,,,,,,,,1.3,0.7,1.1*2B
$GPGSV,4,1,13,02,09,305,24,10,72,161,34,12,19,048,32,21,15,288,18*7A
$GPGSV,4,2,13,23,29,149,22,25,49,084,41,26,16,194,21,28,64,276,42*79
$GPGSV,4,3,13,31,34,245,37,32,59,006,40,194,38,116,32,195,07,158,*77
$GPGSV,4,4,13,199,49,143,*71
$BDGSV,6,1,21,01,36,122,,02,50,223,35,03,56,172,,04,23,111,*6B
$BDGSV,6,2,21,05,29,248,30,06,72,305,38,07,10,171,,08,62,168,*60
$BDGSV,6,3,21,09,66,274,40,10,06,182,,13,72,190,30,14,48,106,45*6D
$BDGSV,6,4,21,16,72,333,42,23,21,214,26,24,47,047,46,25,73,197,34*60
$BDGSV,6,5,21,33,70,069,33,38,49,165,28,39,69,349,40,41,43,326,50*61
$BDGSV,6,6,21,42,22,118,15*51
$GLGSV,2,1,08,78,31,033,25,80,34,196,,79,79,101,44,69,50,001,38*65
$GLGSV,2,2,08,84,10,256,,85,07,306,,67,05,115,,68,43,083,40*69
$GNRMC,041704.000,A,2935.21718,N,10631.58906,E,0.00,172.39,071124,,,A*7E
$GNVTG,172.39,T,,M,0.00,N,0.00,K,A*2D
$GNZDA,041704.000,07,11,2024,00,00*4D
time=1730953024
$GPTXT,01,01,01,ANTENNA OPEN*25
$GNGST,041704.000,62.1,,,,2.7,2.5,5.5*66
$GNGGA,041705.000,2935.21718,N,10631.58906,E,1,25,0.7,299.8,M,-38.7,M,,*69
$GNGLL,2935.21718,N,10631.58906,E,041705.000,A,A*47
$GPGSA,A,3,10,12,25,28,31,32,194,,,,,,1.3,0.7,1.1*07
$BDGSA,A,3,02,05,06,09,13,14,16,23,24,25,33,38,1.3,0.7,1.1*25
$GLGSA,A,3,78,79,69,68,,,,,,,,,1.3,0.7,1.1*2B
$GPGSV,4,1,13,02,09,305,24,10,72,161,34,12,19,048,32,21,15,288,18*7A
$GPGSV,4,2,13,23,29,149,20,25,49,084,41,26,16,194,21,28,64,276,42*7B
$GPGSV,4,3,13,31,34,245,37,32,59,006,41,194,38,116,32,195,07,158,*76
$GPGSV,4,4,13,199,49,143,*71
$BDGSV,6,1,21,01,36,122,,02,50,223,35,03,56,172,,04,23,111,*6B
$BDGSV,6,2,21,05,29,248,30,06,72,305,38,07,10,171,,08,62,168,*60
$BDGSV,6,3,21,09,66,274,41,10,06,182,,13,72,190,30,14,48,106,45*6C
$BDGSV,6,4,21,16,72,333,42,23,21,214,25,24,47,047,46,25,73,197,34*63
$BDGSV,6,5,21,33,70,069,33,38,49,165,27,39,69,349,40,41,43,326,50*6E
$BDGSV,6,6,21,42,22,118,15*51
$GLGSV,2,1,08,78,31,033,31,80,34,196,,79,79,101,44,69,50,001,38*60
$GLGSV,2,2,08,84,10,256,,85,07,306,,67,05,115,,68,43,083,40*69
$GNRMC,041705.000,A,2935.21718,N,10631.58906,E,0.00,172.39,071124,,,A*7F
$GNVTG,172.39,T,,M,0.00,N,0.00,K,A*2D
$GNZDA,041705.000,07,11,2024,00,00*4C
....
```

即：

```shell
time=时间戳
原始nmea报文（按整句行换行）
```

### 环境适配

* 在python 3.10上运行可行，可以试试更高版本的python
* 运行如遇到找不到对应module的编译错误，请在对应python.exe所在路径终端执行`pip install <对应module库>`
  注：如果python配置了系统环境变量也可在终端直接执行`pip install ...`
  注：pip建议更新到最新，如遇下载过慢问题可执行`pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple`（如有更好的镜像源也可使用）

### 运行使用

* 运行`main.py`即可看到如下界面

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/20241220200950.png)

* 选择nmea日志分析的开始时间：输入`nmea.log`中有定位问题或需要分析定位情况的开始时间点，按照如`2024-11-07 11:41:00`的格式输入
* 选择nmea日志分析的结束时间：输入`nmea.log`中有定位问题或需要分析定位情况的结束时间点，输入格式同`选择nmea日志分析的开始时间`
* 选择nmea日志文件路径：点击之后选择本地文件名前缀为`nmea.log`的日志文件
* 选择分析结果输出路径：点击之后选择输出物所在的本地文件路径
* 选择生成图表类型：默认为`散点图`，还可以选择`折线图`，`柱状图`，用于分析`nmea.log`中分析时间段内的可视卫星的SNR分布情况

*散点图示例*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/微信图片_20241223131647.png)

*折线图示例*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/微信图片_20241223131642.png)

*柱状图示例*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/微信图片_20241223131630.png)

* 输入nmea报文（每条以回车分隔）：此为单独分析数据模块，为了分析没有带时间戳的单条或者以换行符分割的多条nmea数据
  * 在下面空白框输入nmea原始报文后点击下方`解析nmea报文`，即可在当前工程所在文件目录下生成`nmeaParse_single`的excel数据表格和用于`图新地图`软件打点的`txt`文件
  * 演示如下：

*使用[NMEA gen](https://nmeagen.org/)网站随机进行打点，并生成NMEA原始报文*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/20241220202851.png)

*以上打点生成的原始NMEA报文如下：*

```
$GPGGA,122213.424,2251.449,N,47420.406,E,1,12,1.0,0.0,M,0.0,M,,*67
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122213.424,A,2251.449,N,47420.406,E,1539.0,181.3,201224,000.0,W*41
$GPGGA,122214.424,2251.022,N,47420.396,E,1,12,1.0,0.0,M,0.0,M,,*67
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122214.424,A,2251.022,N,47420.396,E,1686.9,197.7,201224,000.0,W*4C
$GPGGA,122215.424,2250.576,N,47420.242,E,1,12,1.0,0.0,M,0.0,M,,*6B
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122215.424,A,2250.576,N,47420.242,E,1683.9,204.0,201224,000.0,W*4B
$GPGGA,122216.424,2250.148,N,47420.036,E,1,12,1.0,0.0,M,0.0,M,,*60
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122216.424,A,2250.148,N,47420.036,E,1468.5,207.8,201224,000.0,W*40
$GPGGA,122217.424,2249.788,N,47419.830,E,1,12,1.0,0.0,M,0.0,M,,*67
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122217.424,A,2249.788,N,47419.830,E,1658.9,194.3,201224,000.0,W*48
$GPGGA,122218.424,2249.342,N,47419.706,E,1,12,1.0,0.0,M,0.0,M,,*60
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122218.424,A,2249.342,N,47419.706,E,1472.2,182.7,201224,000.0,W*4D
$GPGGA,122219.424,2248.933,N,47419.685,E,1,12,1.0,0.0,M,0.0,M,,*66
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122219.424,A,2248.933,N,47419.685,E,1199.0,183.3,201224,000.0,W*4C
$GPGGA,122220.424,2248.601,N,47419.665,E,1,12,1.0,0.0,M,0.0,M,,*6C
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122220.424,A,2248.601,N,47419.665,E,1349.6,188.7,201224,000.0,W*40
$GPGGA,122221.424,2248.231,N,47419.603,E,1,12,1.0,0.0,M,0.0,M,,*6A
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122221.424,A,2248.231,N,47419.603,E,1041.9,163.8,201224,000.0,W*48
$GPGGA,122222.424,2247.953,N,47419.691,E,1,12,1.0,0.0,M,0.0,M,,*62
$GPGSA,A,3,01,02,03,04,05,06,07,08,09,10,11,12,1.0,1.0,1.0*30
$GPRMC,122222.424,A,2247.953,N,47419.691,E,1041.9,163.8,201224,000.0,W*40
```

*将NMEA报文放入空白框中*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/20241220203201.png)

*显示解析完成，点击确定之后可以看到当前工程路径下生成了这两个文件*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/20241220203329.png)

*分别打开看一下*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/20241220205258.png)

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/20241221004706.png)



*可以看到解析出了各个NMEA语句经纬度海拔高度和其他信息，接下来使用图新地图打点看看*

![](https://myblog-1308923350.cos.ap-guangzhou.myqcloud.com/img/20241220205439.png)

*打点和网站生成一致*

### 打点地图

本脚本输出打点数据格式为图新地图中txt转kml的txt格式：

```
Longitude,Latitude,Elevation,name
104.0491705,30.655499666666667,0,m1
104.04917016666668,30.655499666666667,0,m2
104.04916999999999,30.655499666666667,0,m3
104.04916983333332,30.655499666666667,0,m4
104.04916966666666,30.655499666666667,0,m5
104.0491695,30.655499499999998,0,m6
104.04916933333334,30.655499333333335,0,m7
104.04916916666667,30.655499333333335,0,m8
104.04916900000002,30.655499333333335,0,m9
104.04916883333333,30.655499499999998,0,m10
104.04916866666666,30.655499666666667,0,m11
104.0491685,30.655499833333337,0,m12
104.04916833333333,30.6555,0,m13
104.04916816666668,30.65550016666667,0,m14
104.04916800000001,30.655500333333332,0,m15
104.04916766666665,30.655500500000002,0,m16
```

<br>

## 输出物

* 用于**图新地图**打点的txt----> `location_data*.txt`
* 包含时间戳和经纬度信息的excel-----> `location*.xlsx`
* 包含时间戳和GPGSV语句解析的卫星信息------> `statellites_data.xlsx`
  包含时间戳和BDGSV语句解析的卫星信息------> `bd_statellites_data.xlsx`
* 如果是在下方空白框输入nmea原始报文解析则会在当前工程目录下生成`nmeaParse_single`的excel和txt，txt用于图新地图打点
