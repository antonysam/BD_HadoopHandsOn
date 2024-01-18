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
## 11. Display only line 11 to 20 from the file in HDFS /tmp/hdfsusecases/NYSE_2020_06.txt
```
hadoop fs -cat /tmp/hdfsusecases/NYSE_2020_06.txt| sed -n 11,20p
```
## 12. Store line 11 to 20 from the file in HDFS /tmp/hdfsusecases/NYSE_2020_06.txt into linux file namely ~/install/hdfsusecases/NYSE_sampledata1.txt
```
hadoop fs -cat /tmp/hdfsusecases/NYSE_2020_06.txt| sed -n 11,20p > ~/install/hdfsusecases/NYSE_sampledata1.txt
```
## 13. Delete the line number 1 from the HDFS file /tmp/hdfsusecases/NYSE_2020_06.txt , for example if the above file contains 100 rows, after deletion it should have only 99 rows in HDFS
Note: we can’t do this directly because of the WORM property of HDFS data, think about the possible work
around and try to achive the result
```
```
## 14. Copy the above file /tmp/hdfsusecases/NYSE_2020_06.txt in the name of /tmp/hdfsusecases/NYSE_2020_06_bkp.txt
```
hadoop fs -cp /tmp/hdfsusecases/NYSE_2020_06.txt /tmp/hdfsusecases/NYSE_2020_06_bkp.txt
```
## 15. Merge the files in HDFS /tmp/hdfsusecases/NYSE_2020_06.txt and /tmp/hdfsusecases/NYSE_2020_06_bkp.txt into Linux directory namely ~/install/hdfsusecases/NYSE_2020_06_merged.txt
Note: We have to use the option called -getmerge to achieve this as given below.
```
```
## 16. Set the blocksize 64MB while writing the file in HDFS, check in the UI how many blocks are generated
```
hadoop fs -Ddfs.blocksize=67108864 -put ~/install/hdfsusecases/NYSE_2020_06.txt /tmp/hdfsusecases/NYSE_2020_05.txt
```
