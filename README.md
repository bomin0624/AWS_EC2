# AWS_EC2
#### Using Hadoop and Apache Spark to analyze big data
```
$ cd ~/spark-1.6.2/ec2
$ ./spark-ec2 -k rrrr -i ~/rrrr.pem -r ap-northeast-1 -z ap-northeast-1a login my-spark-cluster
$ cd spark/bin
$ ./spark-shell --master spark://ip-xxx-xxx-xxxxxx:7077
ssh *slaves' public ip* 
$ more ~/ephemeral-hdfs/conf/masters
$ more ~/ephemeral-hdfs/conf/slaves
$ find ~/ -name "*" -exec grep -H "aaa-aaa-aaa-aaa" {} \; | awk '{FS=":"}{print $1}' > list_master
$ find ~/ -name "*" -exec grep -H "bbb-bbb-bbb-bbb" {} \; | awk '{FS=":"}{print $1}' > list_slaves 
$ cat list_master | xargs -i sed -i 's/aaa-aaa-aaaaaa/ccc-ccc-ccc-ccc/g' {}
$ cat list_slaves | xargs -i sed -i 's/bbb-bbb-bbbbbb/ddd-ddd-ddd-ddd/g' {}  
$ ~/spark-ec2/copy-dir ~/

/*reboot*/

$ ~/ephemeral-hdfs/sbin/start-all.sh
$ ~/tachyon/bin/tachyon-start.sh all Mount
$ ~/spark/sbin/start-all.sh

/*shutdown*/

$ cd ~/spark-1.6.2/ec2

//下載gutenberg 丟到master
$ ./spark-ec2 -k rrrr -i ~/rrrr.pem -r ap-northeast-1 -z ap-northeast-1a login my-spark-cluster

//source命令的作用就是用来執行一個腳本 執行bashrc
$source /~.bashrc 

//瀏覽資料夾
hadoop fs -ls -R 
hadoop fs -ls -R /

//開資料夾
hadoop fs -mkdir /路徑名稱 
hadoop fs -copyFromLocal ~/上傳路徑/98-0.txt /路徑名稱
hadoop fs -copyFromLocal ~/98-0.txt /bomin
cd spark/bin
./spark-shell --master spark://ip-xxx-xxx-xxxxxx:7077 //Enter spark 

val textFile = sc.textFile("hdfs://ec2-54-238-146-164.ap-northeast-1.compute.amazonaws.com:9000/bomin/RAJ.txt")
val stringRDD=textFile.
    flatMap(line => line.split(" "))

val countsRDD = stringRDD.
                map(word => (word, 1)).
                reduceByKey(_ + _)
countsRDD.saveAsTextFile("hdfs://ec2-54-238-146-164.ap-northeast-1.compute.amazonaws.com:9000/bomin/output")
:q  //quit
hadoop fs -ls -R  //瀏覽資料夾
hadoop fs -cat /20170111/output/part0000 //open the book file
```