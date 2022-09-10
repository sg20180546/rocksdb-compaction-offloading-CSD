# Rocksdb version 7.3.1 for "Compaction" code hacking

# Objective : alleviate CPU and Disk IO Bottleneck by CSD-SSD (Computational Storage Drive)

## To do (22/09/10)
### 1. Hack Function AddToBuilder (db/compaction/compaction_job.cc) (clear)
### 2. Hack class ColumnFamilyData (Priority : MID)
### 3. Research about Remote Compaction (Priority : High) (clear)
### <s>4. Hack LevelCompactionPicker::NeedCompaction (Priority : Low) (aborted) </s>
### 5. Make User Level MultiThreading Middleware(Primary DB in Host - Secondory DB in CSD) in CSD (Priority : HIGH) (clear)
### 6. Config CSD emulation environment with 2 Computer(limit of high-speed-conecting of Newport CSD)


# Overall Structure
![image](https://user-images.githubusercontent.com/81512075/187039960-ae78d32d-faef-47e3-ad59-5eb45bbbadd9.png)



## Command
make -j 80 db_bench DEBUG_LEVEL=0

./db_bench -max_background_jobs=96 -value_size=4096 -histogram -benchmarks="fillrandom,stats" -statistics -perf_level=3 -compression_ratio=1 -db={sstfilepath} -threads=60 -use_direct_io -num=349525

## Host side :: ProcessKeyValueCompaction Flow
![image](https://user-images.githubusercontent.com/81512075/187040258-f8e884b0-2bbb-4eb6-86e1-d4f259175105.png)

## DB side : CompactionThread Flow
![image](https://user-images.githubusercontent.com/81512075/187039761-67fe6d27-603d-4b13-ba29-0ed759fd31d5.png)


---------------------------------------




## DBImpl::BGWorkCompaction Flow
![image](https://user-images.githubusercontent.com/81512075/182875447-73b0da8f-9ac1-4229-be4c-6ae24af550c0.png)

## CompactionJob::ProcessKeyValueCompaction Flow
![image](https://user-images.githubusercontent.com/81512075/187039658-162d7484-01fc-43ab-b109-61df9bf8988f.png)

## LevelCompactionPicker::NeedCompaction Flow 
![image](https://user-images.githubusercontent.com/81512075/182875334-e9c8bc91-a5fb-4e35-89df-cee8e1644aee.png)

