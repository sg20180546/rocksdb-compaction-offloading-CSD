# Research Objective : alleviate CPU and IO bandwidth bottleneck by CSD (Computational Storage Drive)

## To do (22/09/10)
### 1. hack Function AddToBuilder (db/compaction/compaction_job.cc) (clear)
### 2. hack class ColumnFamilyData (Priority : MID)
### 3. research about Remote Compaction (Priority : High) (clear)
### <s>4. hack LevelCompactionPicker::NeedCompaction (Priority : Low) (aborted) </s>
### 5. make User Level MultiThreading Middleware(Primary DB in Host - Secondory DB in CSD) in CSD (Priority : HIGH) (clear)
### 6. config CSD emulation environment with 2 Computer(limit of high-speed-conecting of state-of-art Newport CSD) (Priority : HIGH)
### 6-1 construct PCIe Tunneling,Ethernet,NFS between Host and CSD (clear)
- connect lan line , set network option ( Address and Netmask )
- install nfs-common at client // nfs-common, nfs-kernel-server, rpcbind and portmap at server 
- edit /etc/exports // exportsfs -a at server
- mount N.N.N.N:/{server_dir_path} {client_dir_path} 
- network sharing
### 6-2. evaluate some indicator (iperf,sperf, io bandwidth ...)
Indicator	HOST	CSD	
OS	Ubuntu 20.04.3 LTS	Ubuntu 20.04.4	
kernel	 5.15.0-48	5.13.0-51	
CPU	Intel(R) Core(TM) i7-4790 CPU @ 3.60GHz Quad core	Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz Quad core	
DRAM	16GB	32GB	
SSD	Samsung SSD 970 EVO 250GB		
SECTOR SIZE	512 BYTES		
I/O SIZE	512 BYTES		
Mount on	/csd	/dev/nvme0n1: /csd	overprovisioned(229GB)
Ethernet	1Gbp/S		
Iperf3	117MB/s		
Ethernet ip	10.42.0.1	10.42.0.2	
Indicator	HOST	CSD	
OS	Ubuntu 20.04.3 LTS	Ubuntu 20.04.4	
kernel	 5.15.0-48	5.13.0-51	
CPU	Intel(R) Core(TM) i7-4790 CPU @ 3.60GHz Quad core	Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz Quad core	
DRAM	16GB	32GB	
SSD	Samsung SSD 970 EVO 250GB		
SECTOR SIZE	512 BYTES		
I/O SIZE	512 BYTES		
Mount on	/csd	/dev/nvme0n1: /csd	overprovisioned(229GB)
Ethernet	1Gbp/S		
Iperf3	117MB/s		
Ethernet ip	10.42.0.1	10.42.0.2	
![image](https://user-images.githubusercontent.com/81512075/192194314-92176f36-68ed-4838-a893-5a4242890c5b.png)


<img src="https://user-images.githubusercontent.com/81512075/192192816-fb60fb97-dee7-4f33-bb3d-0a859453447f.png" width="50%" height="30%"> </img>
<img src="https://user-images.githubusercontent.com/81512075/192192949-9bedfc0e-8a06-49dc-a6e1-b8128068d3c7.png" width="50%" height="30%"> </img>


### 6-3. boot rocksdb on Host, rocksdb-secondary on CSD
![image](https://user-images.githubusercontent.com/81512075/189485360-a62ab75d-b0bb-42a3-86e5-8bb61686530a.png)

# Overall Structure
![image](https://user-images.githubusercontent.com/81512075/187039960-ae78d32d-faef-47e3-ad59-5eb45bbbadd9.png)
## Key Component
![image](https://user-images.githubusercontent.com/81512075/189485259-3d61dcf1-cdfb-464d-925e-6476d9e878a6.png)




## Command
make -j 80 db_bench DEBUG_LEVEL=0

./db_bench -max_background_jobs=96 -value_size=4096 -histogram -benchmarks="fillrandom,stats" -statistics -perf_level=3 -compression_ratio=1 -db={sstfilepath} -threads=60 -use_direct_io -num=349525

## Host side :: ProcessKeyValueCompaction Flow
![image](https://user-images.githubusercontent.com/81512075/187040258-f8e884b0-2bbb-4eb6-86e1-d4f259175105.png)

## DB side : CompactionThread Flow
![image](https://user-images.githubusercontent.com/81512075/187039761-67fe6d27-603d-4b13-ba29-0ed759fd31d5.png)


---------------------------------------
---------------------------------------





# Code Hacking


## DBImpl::BGWorkCompaction Flow
![image](https://user-images.githubusercontent.com/81512075/182875447-73b0da8f-9ac1-4229-be4c-6ae24af550c0.png)

## CompactionJob::ProcessKeyValueCompaction Flow
![image](https://user-images.githubusercontent.com/81512075/187039658-162d7484-01fc-43ab-b109-61df9bf8988f.png)

## LevelCompactionPicker::NeedCompaction Flow 
![image](https://user-images.githubusercontent.com/81512075/182875334-e9c8bc91-a5fb-4e35-89df-cee8e1644aee.png)

## Options
![image](https://user-images.githubusercontent.com/81512075/189485315-6e89214e-487b-46a4-8c4c-6de9f7c17be8.png)

