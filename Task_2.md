# 2. Hadoop Cluster Sizing
We will estimate the size of the platform of a Hadoop infrastructure that processes movie events from various sources (cinemas, streaming platforms, etc.). Here is the expected data volume:

| **Sources** | **Average Events** | **Size per Event** |
| --- | --- | --- |
| Source 1 | 10,000 events/day | 15 KB |
| Source 2 | 120,000 events/day | 300 Bytes |
| Source 2 | 150,000 events/day | 100 KB |
| Source 2 | 170,000 events/day | 800 KB |
| Source 2 | 2,000 events/day | 1500 KB |

Machine Characteristics: up to 22 disks, 2 Terabytes per disk

**Number of machines needed to store all the data volume over the next year and justification:**

There are several ways to decide how many machines are required for a Hadoop implementation. The most common one is to size the cluster based on the required storage amount. Another less common option would be to project the cluster size based on the completion time of specific jobs, which makes sense in certain specific circumstances. In this case, we assume that we are working with the first option: **sizing the cluster based on the required storage amount**.

The formula we will use to estimate Hadoop storage (H) is as follows:

H = c \* r \* S / (1 - i)

where:

- **c = average compression ratio**. It depends on the type of compression used (Snappy, LZOP, ...) and the data size. When no compression is used, c = 1.
- **r = replication factor**. Normally it's 3 in a production cluster.
- **S = data size to be moved to Hadoop**. This could be a combination of historical data and incremental data. Incremental data could be daily, for example, and projected over a period of time (e.g., 1 year in our case).
- **i = intermediate factor**. Normally it's 1/3 or 1/4. Hadoop workspace dedicated to storing intermediate results of Map phases.

This formula was derived by Slim Baltagi (Director of Big Data Engineering at Capital One), and after cross-referencing it with several sources, we've seen that it's widely accepted in the industry.

As for our case, it doesn't mention the compression ratio, so we will consider that no compression is used, i.e., c = 1. Additionally, since it doesn't specify any specific replication factor, we assume it to be 3, i.e., r = 3. In a real-world use case, we would need to validate these characteristics with the requester before proceeding with the final decision.

Regarding the data size S, we will calculate the total volume of data generated per year by both sources:

| **Source** | **Events/day** | **Size/event (KB)** | **Total size (KB/year)** | **Total size (TB/year)** |
| --- | --- | --- | --- | --- |
| Source 1 | 10,000 | 15 | 54,750,000 | 0.0510 |
| Source 2 | 120,000 | 0.2930 | 12,832,031.25 | 0.0120 |
| Source 2 | 150,000 | 100 | 5,475,000,000 | 5.0990 |
| Source 2 | 170,000 | 800 | 49,640,000,000 | 46.2309 |
| Source 2 | 2,000 | 1,500 | 1,095,000,000 | 1.0198 |
|     |     |     | **Total Sum (TB/year)** | **52.4126** |

The operations performed were:

- Conversion of 300 Bytes to Kilobytes: 300/1024
- Calculation of the column "Total size (KB/year)": Events/day \* Size/event (KB)
- Calculation of the column "Total size (TB/year)": Total size (KB/year) / (1024\*1024)
- Calculation of "Total Sum (TB/year)": Summation of the column "Total size (TB/year)"

Therefore, S = 52.4126 TB.

Finally, as for the intermediate factor, it represents the space we need to leave on each data node to process the data locally. It's typically recommended to be 25%, so i = 0.25.

Therefore, our required Hadoop storage will be:

| **Factors** | **Values** |
| --- | --- |
| c   | 1   |
| r   | 3   |
| S   | 52.4126 |
| i   | 0.25 |

**H =** c \* r \* S / (1 - i) = 1 \* 3 \* 52.4126 / (1-0.25) = **209.6504 TB**

Considering that our machines have a capacity of up to 22 disks of 2 Terabytes each, each machine could store up to 44 TB of capacity. Therefore:

⌈209.6504 TB / 44 TB/machine⌉ = ⌈4.76 machines⌉ = **5 machines**

