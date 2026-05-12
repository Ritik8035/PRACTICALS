BDA_PRACTICAL_1


Hadoop MapReduce Practical – Complete Steps

Step 1: Start Hadoop
start-dfs.sh

Step 2: Check Hadoop Processes
jps

Expected Output:
NameNode
DataNode
SecondaryNameNode
Jps

Step 3: Create Practical Folder
mkdir ~/practical1
cd ~/practical1

Step 4: Create mapper.py
command - nano mapper.py

#!/usr/bin/env python3
import sys

for line in sys.stdin:
    line = line.strip()
    words = line.split()

    for word in words:
        print(f"{word}\t1")

Step 5: Create reducer.py
command - nano reducer.py

#!/usr/bin/env python3
import sys

current_word = None
current_count = 0

for line in sys.stdin:
    word, count = line.strip().split('\t')
    count = int(count)

    if current_word == word:
        current_count += count
    else:
        if current_word:
            print(f"{current_word}\t{current_count}")

        current_word = word
        current_count = count

if current_word == word:
    print(f"{current_word}\t{current_count}")

Step 6: Create input.txt
command - nano input.py


hello world
welcome to the world of big data
hello

Step 7: Give Permission
chmod +x mapper.py reducer.py

Step 8: Create HDFS Input Folder
hadoop fs -mkdir /input

Step 9: Upload Input File
hadoop fs -put input.txt /input

Step 10: Run MapReduce Program

hadoop jar ~/hadoop/share/hadoop/tools/lib/hadoop-streaming-3.3.6.jar \
-files mapper.py,reducer.py \
-mapper mapper.py \
-reducer reducer.py \
-input /input/input.txt \
-output /output

Step 11: View Output
hadoop fs -cat /output/part-00000

Expected Final Output

big 1
data 1
hello 2
of 1
the 1
to 1
welcome 1
world 2

Important While Re-running
hadoop fs -rm -r /output
