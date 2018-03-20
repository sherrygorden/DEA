# Read data for network DEA function
*POLab*
<br>
*2018/03/20*
<br>
[【回到首頁】](https://github.com/wurmen/DEA)
<br>
#### ※*Reference*
*本篇範例資料取自[高強教授](http://www.iim.ncku.edu.tw/files/11-1407-20368.php?Lang=zh-tw)於2007發表的Paper：[Efficiency decomposition in network data envelopment analysis: A relational model](https://www.sciencedirect.com/science/article/pii/S0377221707010077)*

------------

由於Network DEA涉及了決策單位的內部製程(processes)，整體系統以及每個製程都有各自的投入與產出，加上有些製程的產出會成為部分製程的投入，因此，有別於[基本DEA function的讀檔函式](https://github.com/wurmen/DEA/blob/master/Functions/readdata_function.md)，在此對程式的部份內容進行了修改，以利Network DEA function的使用。<br>
在本文中，會對讀檔的函式進行參數及使用說明，並於最後舉兩個例子解說資料要如何給定，對於使用Network DEA function，資料的給定相當重要，因此可能要多花點時間閱讀最後的兩個[範例說明](#Example)。

## csv2dict_for_network_dea()

### § Description
- 用來讀取各決策單位整體系統及各製程投入與產出資料的csv檔，將資料轉換成字典格式(dictionary type)，以利後續建模使用。
### § Usage
- csv2dict_for_network_dea(dea_data, v1_range, v2_range, p_n=0)
- 回傳四個值，分別是DMU(list), V1(dict), V2(dict),p_n(interger)

### § Arguments
- **dea_data**：string, 資料所在的路徑位置(path)，必須是csv檔案
- **v1_range**：list, 整體系統及各製程中，來自外部的投入資料 **(X)** 的行範圍 (或者整體系統及各製程中，為系統最終產出 **(Y)** 的行範圍)
- **v2_range**：list, 整體系統及各製程中，該投入為來自某內部製程產出資料 **(Z_input)** 的行範圍 (或者整體系統及各製程中，該產出將成為某內部製程投入資料 **(Z_output)** 的行範圍)
- **p_n**：interger, 內部製程的數量

### § Notice
- 檔案必須為csv格式，**資料從第一行的第二列開始讀起**，並且首行必須為DMU及各製程的名稱，首列可為各產出投入資料的名稱，如Example所示
- 檔案內數值不能包含逗號

### § <h2 id="Example">§ Example</h2>
#### 1.說明
為了簡化Network DEA function的複雜度，在此偏向給定較完整的整體資料來對主函式做使用，首先，必須先釐清整體系統及各製程投入與產出的角色，在Network DEA function中，主要將投入項分為X、Z_input兩個角色，產出項分為Y、Z_output兩個角色，各角色說明如下：<br>
- **X**：來自系統外部的投入
- **Z_input**：該投入是來自某內部製程的產出，在此被視為中間產物的投入項
- **Y**：會成為系統最終產出的產出項
- **Z_output**：只要該產出有部分會成為某內部製程的投入，即被視為中間產物的產出項

#### 2.資料給定建議
建議將X與Z_input兩項資料放置同一csv檔內，形成投入資料，Y與Z_output放在同一csv檔，形成產出資料，再各自透過函式csv2dict_for_network_dea()讀取，回傳X、Z_input及Y、Z_output的字典格式

#### 3.範例
#### Example1

**題目描述**
- 下圖是一個由三個製程所形成的系統，該系統最初有兩項投入，最終會有三項產出，而系統內部各製程的投入與產出情形，如下圖所示：<br>
1. 系統最初的兩項投入會被分成三個部份分別給製程1、製程2及製程3當成他們各自的投入項。<br>
2. 製程1和製程2的產出會被分成兩個部份，一部份為最終系統產出，另一部份則當成製程3的部份投入項

<div align=center>
<img src="https://github.com/wurmen/DEA/blob/master/Network_DEA/pictures/network%20system.PNG" width="550" height="350">
</div>
<br>

**角色釐清**
- **X**：X</sub><sub>1</sup></sub>、X</sub><sub>2</sup></sub>
- **Z_input**：Y</sub><sub>1</sup></sub>、Y</sub><sub>2</sup></sub> (對製程3而言)
- **Y**：Y</sub><sub>3</sup></sub>
- **Z_output**：Y</sub><sub>1</sup></sub>、Y</sub><sub>2</sup></sub> (對製程1及製程2而言)

**原始產出與投入項數據**
- 共有五個決策單位要進行比較，其系統及各製程的產出與投入情形如下表所示：<br>

※ Y<sup>(O)</sup></sub><sub>1</sup></sub>、Y<sup>(I)</sup></sub><sub>1</sup></sub>：Y<sup>(O)</sup></sub><sub>1</sup></sub>為製程1最終的系統產出，Y<sup>(I)</sup></sub><sub>1</sup></sub>為製程1會成為製程3的部份投入項<br>

※ Y<sup>(O)</sup></sub><sub>2</sup></sub>、Y<sup>(I)</sup></sub><sub>2</sup></sub>： Y<sup>(O)</sup></sub><sub>2</sup></sub>為製程2最終的系統產出，Y<sup>(I)</sup></sub><sub>2</sup></sub>為製程2會成為製程3的部份投入項

<div align=center>
<img src="https://github.com/wurmen/DEA/blob/master/Network_DEA/pictures/example-data.PNG" width="800" height="370">
</div>
<br>

**給定資料型式**








