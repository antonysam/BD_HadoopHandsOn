## 1. Create a new directory in linux namely ~/install/hdfsusecases and create a new file inside the above directory namely ~/install/hdfsusecases/NYSE_2020_06_20.txt copying the first 1000 lines from an existing file
## ~/pigdata/NYSE_daily
```
mkdir ~/install/hdfsusecases
>> ~/install/hdfsusecases/NYSE_2020_06_20.txt
head -1000 NYSE_daily >> ~/install/hdfsusecases/NYSE_2020_06_20.txt
```
## 2. Create another new file inside the above directory namely ~/install/hdfsusecases/NYSE_2020_06_21.txt copying the line from 1001 to 2000 from an existing file ~/pigdata/NYSE_daily
```
> ~/install/hdfsusecases/NYSE_2020_06_21.txt

head -2000 NYSE_daily | tail -1000
or
awk 'NR>=1001 && NR<=2000 {print}' NYSE_daily
or
sed -n 1001,2000p NYSE_daily
```
## 3. Create a directory in Hadoop namely /tmp/hdfsusecases
```
hadoop fs -mkdir /tmp/hdfsusecases
```
## 4. Check whether the above directory is created in HDFS or not using the below command (Note: We use –test –d option to check whether the given path is a directory or not)
```
hdfs dfs -find /tmp/hdfsusecases
```
## 5. Check what is the status code of the above command using, if it shows 0 then directory is created, if shows non zero then the directory is not created then check the step 3 again.
```
echo $?
```
## 6. Copy file generated only in step 1 (~/install/hdfsusecases/NYSE_2020_06_20.txt) from linux to hdfs directory /tmp/hdfsusecases in the name of NYSE_2020_06.txt
```
hadoop fs -put ~/install/hdfsusecases/NYSE_2020_06_20.txt /tmp/hdfsusecases/NYSE_2020_06.txt
```
## 7. Like step 4 and 5, check whether the above file (/tmp/hdfsusecases/NYSE_2020_06.txt) is created or not in HDFS, using -f option and check for the status code
## using $? and create a zero byte file in HDFS directory
## /tmp/hdfsusecases in the name of _SUCCESS
```
hadoop fs -test -f /tmp/hdfsusecases/NYSE_2020_06.txt
echo $?
Note: If 0 then it is success else not created
hadoop fs -touchz /tmp/hdfsusecases/_SUCCESS
```
## 8. Append the file generated in step 2 in linux (~/install/hdfsusecases/NYSE_2020_06_20.txt) with the file generated in step 6 in the hdfs directory /tmp/hdfsusecases/NYSE_2020_06.txt
```
hadoop fs -appendToFile ~/install/hdfsusecases/NYSE_2020_06_20.txt /tmp/hdfsusecases/NYSE_2020_06.txt
```
## 9. Count the size of the file in HDFS /tmp/hdfsusecases/NYSE_2020_06.txt
```
hadoop fs -count /tmp/hdfsusecases/NYSE_2020_06.txt
```
## 10. Count the number of rows are there in the /tmp/hdfsusecases/NYSE_2020_06.txt (Which should show the total count of the files created in step1 and 2)
```
hadoop fs -cat /tmp/hdfsusecases/NYSE_2020_06.txt | wc -l
```
