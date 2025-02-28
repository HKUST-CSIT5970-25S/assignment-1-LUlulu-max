[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: LI Lu
### Student Id: 21076155
### Email: llien@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    I used Phoronix Test Suite during the measurements.
    > 
    **> Configuration:**
    > Installation：
    ```
    ssh -i /Users/lilu/Downloads/best.pem ubuntu@ec2-3-84-88-156.compute-1.amazonaws.com
    sudo apt update
    sudo apt install php-cli php-xml
    wget https://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10.8.4_all.deb
    sudo dpkg -i phoronix*.deb
    phoronix-test-suite
    sudo apt-get install php-zip
    ```
    > Test Commands：
    ```
    phoronix-test-suite run pts/compress-7zip
    phoronix-test-suite run pts/ramspeed
    ```
    >
    - `compress-7zip` is a test suite and it could evaluate the performance of `7-Zip` when compressing and decompressing files. It involves metrics such as compression speed, decompression speed, and compression ratio. Here I use the average compression rating to evaluate the CPU performance.
      
    - `ramspeed` is a tool specifically designed to test the performance of computer memory. It measures performance of the read and write speed by executing a series of specific memory operations, such as sequential read and write, and random read and write.
    
    **> Explanation of Measurement Results:**
     - CPU performance： The measurement results of CPU performance are MIPS (Million Instructions Per Second). The higher the MIPS, the better the CPU performance.
    
    - Memory performance: The measurement results of memory performance are in MB/s, which indicates the speed of data transfer rate. The higher the value, the better the memory performance.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |    3494 MIPS           |      10376.01 MB/s             |
    | `t2.medium`  |   9673 MIPS            |     19328.82 MB/s              |
    | `c5d.large` |    7641 MIPS            |     13508.68 MB/s              |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    
    
    The performance of EC2 instances does not increase commensurate with the increase of the number of vCPUs and memory resource. The details are as follows:

   |     Instance Type                 | vCPU | Memory |
    | ------------------------- | -------------- | -------- |
    | `t2.micro` |        1 vCPU        |   1 GiB Memory       |
    | `t2.medium`  |       2 vCPU        |   2 GiB Memory      |
    | `c5d.large` |        2 vCPU       |  4 GiB Memory      |
   
    - Comparing `t2.medium` and `c5d.large`, when the memory increases twice from 2 GiB to 4 GiB while the number of vCPU stays the same, the CPU performance decreases by 21% and the Memory performance decreases by 30%.
   
    - Comparing `t2.micro` and `t2.medium`, when the number of vCPUs increases from 1 to 2 and the memory increases from 1 GiB to 2 GiB, the CPU performance increases by 176% and the Memory performance increases by 86%.
   
   - Comparing `t2.micro` and `c5d.large`, when the number of vCPUs increases from 1 to 2 and the memory increases from 1 GiB to 4 GiB, the CPU performance increases by 118% and the Memory performance increases by 30%.

   Above all, the performance of EC2 instances generally increases with the increase in the number of vCPUs and memory resources. But this growth is not proportional.
   This is because memory performance is not solely determined by memory size; it is also affected by various factors such as memory type, system architecture, and memory management strategies. An increase in memory size may also be accompanied by optimizations in other aspects (such as storage I/O performance), which will affect the performance of memory to some extent. An increase in the number of vCPUs will improve both CPU and memory performance. However, an increase in memory size will lead to a much smaller improvement in memory performance compared to the improvement in CPU performance. 

   

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |   3940             |    0.241(Average)      |
    | `m5.large` - `m5.large`   |   4950             |   0.155(Average)       |
    | `c5n.large` - `c5n.large` |   4960             |   0.145(Average)       |
    | `t3.medium` - `c5n.large` |   2440             |   0.642(Average)       |
    | `m5.large` - `c5n.large`  |   4960             |   0.125(Average)       |
    | `m5.large` - `t3.medium`  |   2350             |   0.652(Average)       |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.
    
Comparing the instances of different types with the instances of the same types within the same region, the TCP bandwidth generally decreases, and the round-trip time(RTT) generally increases, which indicates that the network performance between the same types of instances is better than that between different types of inatances within the same region. 

The possible reasons are that there is poor coordination in the configurations between instances of different types, resource allocation is likely to be unbalanced, and there may be compatibility issues.


2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |     32.7       |  63.890(Average)  |
    | N. Virginia - N. Virginia |     4960       |  0.265(Average)        |
    | Oregon - Oregon           |     4960       |   0.136(Average)       |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
    
Compared with instances within the same region, the TCP bandwidth between different regions is significantly reduced and the average RTT increases significantly. This is because when data is transmitted between different regions, it needs to pass through more network nodes and a longer physical distance, which increases the signal transmission delay. At the same time, the network topology between different regions is more complex, which may introduce more routing hops and network congestion, further increasing the delay.




    
