## Create a new directory in linux namely ~/install/hdfsusecases and create a new file inside the above directory namely ~/install/hdfsusecases/NYSE_2020_06_20.txt copying the first 1000 lines from an existing file
## ~/pigdata/NYSE_daily
```
mkdir ~/install/hdfsusecases
>> ~/install/hdfsusecases/NYSE_2020_06_20.txt
head -1000 NYSE_daily >> ~/install/hdfsusecases/NYSE_2020_06_20.txt
```
## Create another new file inside the above directory namely ~/install/hdfsusecases/NYSE_2020_06_21.txt copying the line from 1001 to 2000 from an existing file ~/pigdata/NYSE_daily
```
head -2000 NYSE_daily | tail -1000
or
awk 'NR>=1001 && NR<=2000 {print}' NYSE_daily
or
sed -n 1001,2000p NYSE_daily
```
## Create a directory in Hadoop namely /tmp/hdfsusecases
```
hadoop fs -mkdir /tmp/hdfsusecases
```
## Check whether the above directory is created in HDFS or not using the below command (Note: We use –test –d option to check whether the given path is a directory or not)
```
hdfs dfs -find /tmp/hdfsusecases
```



