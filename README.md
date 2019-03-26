# Apache Spark

## Hadoop vs. Spark
![hadoop_vs_spark](/images/hadoop_vs_spark.PNG)

## Speed
根據 Apache Spark 官方網站的說明，Spark 在記憶體內執行運算時，最快可以比 Hadoop MapReduce 快100倍。即使與 MapReduce 一樣將運算結果儲存在硬碟上，運算速度也可以快上10倍。Spark 是基於記憶體內的計算框架。Spark 在運算時，將中間產生的資料暫存在記憶體中，因此可以加快執行速度。尤其需要反覆操作的次數越多，所需讀取的資料量越大，則越能看出 Spark 的效能，而 Hadoop 每次做完一次運算就必須做硬碟 I/O。

## 架構
![architecture](/images/architecture.png)

## 運算方式
Apache Spark 是一個分散式的運算框架 (Framework)，可分為以下幾種執行運算的方法，後面的文章會介紹這幾種執行方式的方法與差別。
<ul>
<li>local mode</li>
<li>Standalone</li>
<li>On Hadoop Yarn</li>
<li>On Apache Mesos</li>
</ul>

## Components
Spark 主要包含四個函式庫的功能：
<ul>
<li>Spark SQL</li>
<li>Spark Streaming</li>
<li>MLlib (machine learning)</li>
<li>GraphX (graph)</li>
</ul>

# RDD
Spark 的核心是 RDD，Resilient Distributed DataSet 的縮寫，是一種具有容錯 (tolerant) 與
高效能 (efficient) 的抽象資料結構。RDD 由一到數個的 partition 組成， Spark 程式進行運算時，
partition 會分散在各個節點進行運算，預設會被存放在記憶體內，所以可以快速分享各個 partition 的運算結果，
但若記憶體不足會出現 OOM Exception 錯誤訊息，可透過參數設定存放在硬碟避免發生該錯誤。

Spark 是由 Scala 撰寫而成，嚴格遵守 Functional Program 的概念，所以RDD只能讀取無法寫入。Spark 支援讀取 HDFS 等分散式儲存裝置的檔案，故可以使用 HDFS 的特性便於進行分散式的運算。

RDD 具有的特性：Immutable, Distributed, Parallelizing

## Example: 讀取HDFS檔案，將內容轉成小寫並且以空白為分割符號將每個字切開，對文字與數字分別進行計算
![wordcount](/images/wordcount.PNG)
![rdd_lineage](/images/rdd_lineage.png)

## Example: wordcount with spark shell
![shell](/images/shell1.PNG)
![shell](/images/shell2.PNG)
(計算README.md內以行(row)為比較單位，每行最多有幾個字)
![shell](/images/shell3.PNG)
(列印出RDD內容)
![shell](/images/shell4.PNG)

## Example: mapreduce
![mapreduce](/images/mapreduce.PNG)

## Example: spark-submit | Spark Hellow World API
![spark-submit](/images/spark-submit1.PNG)
![spark-submit](/images/spark-submit2.PNG)
![spark-submit](/images/spark-submit3.PNG)

## Spark SQL
Spark SQL 是 Spark 用來執行 SQL 語法查詢的一種功能，也支援 HiveQL 查詢語法，可透過 Spark application 撰寫程式對使用 Hive 建立的 Table 進行查詢。

使用 Spark SQL 回傳的物件類型是 DataFrame，是一種用來命名欄位的分散式資料集合。概念有點像優化版本的 RDB table，可以接受更多的資料來源建立 DataFrame，例如結構化的的檔案 (CSV)、Hive Table、外部資料庫或是 Spark RDD。

DataSet 在 Spark 1.6 版本所提出，想藉由 Spark SQL 的優化引擎來強化 RDD的 優勢，可以想像成是加強版的 DataFrame。DataSet 提供強型別 (strong type) 與 lambda functions。其中強型別的好處是在編譯時就可以發現資料型態是否正確而提出警告，有別於傳統 RDD 必須要在執行期 (runtime) 才能發現，可以儘早改善錯誤。

## Spark Streaming
Spark Streaming 是以 Spark 核心 API 擴充出來的一個模組，他在處理資料串流 (streaming) 上具有可擴充性、高吞吐量、高容錯性特點。可以從 Kafka，Flume，Kinesis 或 TCP 等許多來源介接資料，也可以透過 Spark API 的 map，reduce，join 和 window 等函數進行複雜的運算來處理資料。最後再將運算結果送到檔案系統 (如 HDFS)、資料庫或是即時的監控系統，也可以將資料餵給機器學系的系統，進行即時的運算。

Spark Streaming 提供了一個高層級的抽象層，稱之為 discretized stream (DStream)，意指連續的資料串流。DStream 可以通過 Kafka，Flume 和 Kinesis 等資料來源來建立，也可以通過在其他 DStream 的 API 來新增。在 Spark Streaming 中，一個 DStream 即為一種有順序的 RDD。

## More
Spark Shell 可以讓使用者快速了解如何撰寫 RDD 並得到結果，不適用於大量運算。如果需要計算大量資料，建議撰寫 Scala 或是 Java 程式，編譯後打包並使用 spark-submit 指令送至資源管理系統進行分散式計算，效能會比較好。


## Reference
[https://ithelp.ithome.com.tw/articles/10192216](https://ithelp.ithome.com.tw/articles/10192216)
[https://medium.com/@kstseng/spark-%E8%B6%85%E5%85%A5%E9%96%80%E7%AD%86%E8%A8%98-6d06911cdbdf](https://medium.com/@kstseng/spark-%E8%B6%85%E5%85%A5%E9%96%80%E7%AD%86%E8%A8%98-6d06911cdbdf)