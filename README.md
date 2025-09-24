# Advanced Databases - Spark Crime Data Analysis

## 📊 Project Overview
This project implements a comprehensive analysis of Los Angeles crime data (2010-2023) using **Apache Spark** on a distributed **Hadoop cluster**. The system processes over **2.1 million crime records** to extract meaningful insights about crime patterns, temporal trends, and geographic distributions using big data technologies.

**Repository**: [https://github.com/evigiann/spark-crime-data-analysis](https://github.com/evigiann/spark-crime-data-analysis)


## 🏗️ Technical Architecture
- **Cluster Setup**: 2-node Hadoop cluster with HDFS and YARN resource management
- **Processing Engine**: Apache Spark 3.4+ with PySpark
- **Data Volume**: 2,135,495 crime records processed
- **APIs Used**: DataFrame API, SQL API, RDD API
- **Storage**: Hadoop Distributed File System (HDFS)

## 📈 Dataset Schema
The crime dataset contains 28 columns with the following structure:
```
root
 |-- DR_NO: string (nullable = true)
 |-- Date Rptd: date (nullable = true)
 |-- DATE OCC: date (nullable = true)
 |-- TIME OCC: string (nullable = true)
 |-- AREA: string (nullable = true)
 |-- AREA NAME: string (nullable = true)
 |-- Rpt Dist No: string (nullable = true)
 |-- Part 1-2: string (nullable = true)
 |-- Crm Cd: string (nullable = true)
 |-- Crm Cd Desc: string (nullable = true)
 |-- Mocodes: string (nullable = true)
 |-- Vict Age: integer (nullable = true)
 |-- Vict Sex: string (nullable = true)
 |-- Vict Descent: string (nullable = true)
 |-- Premis Cd: string (nullable = true)
 |-- Premis Desc: string (nullable = true)
 |-- Weapon Used Cd: string (nullable = true)
 |-- Weapon Desc: string (nullable = true)
 |-- Status: string (nullable = true)
 |-- Status Desc: string (nullable = true)
 |-- Crm Cd 1: string (nullable = true)
 |-- Crm Cd 2: string (nullable = true)
 |-- Crm Cd 3: string (nullable = true)
 |-- Crm Cd 4: string (nullable = true)
 |-- LOCATION: string (nullable = true)
 |-- Cross Street: string (nullable = true)
 |-- LAT: double (nullable = true)
 |-- LON: double (nullable = true)
```

**Total number of rows: 2,135,495**

## 🔍 Analytical Queries & Results

### Query 1: Top Crime Months by Year
**Objective**: Find the top 3 months with highest crime counts for each year

**Implementation**: DataFrame API vs SQL API with 4 Spark executors

**Performance Results**:
| Implementation | Execution Time (Seconds) |
|----------------|--------------------------|
| DataFrame API  | 13.726                   |
| SQL API        | 9.9                      |

**Key Insight**: SQL API outperformed DataFrame API by approximately 28%


### Query 2: Street Crimes by Time of Day
**Objective**: Analyze street crime distribution across different parts of the day

**Time Categories**:
- **Morning**: 5:00 AM - 11:59 AM
- **Afternoon**: 12:00 PM - 4:59 PM  
- **Evening**: 5:00 PM - 8:59 PM
- **Night**: 9:00 PM - 3:59 AM

**Results**:
```
PartOfDay   count
Night       688,121
Morning     5,038
Afternoon   1,668
Evening     1,031
```

**Performance Comparison**:
| Implementation | Execution Time (Seconds) |
|----------------|--------------------------|
| DataFrame API  | 6.30                     |
| SQL API        | 9.45                     |
| RDD API        | 31.76                    |

**Key Insight**: DataFrame API was most efficient, while RDD API was significantly slower due to lack of optimization


### Query 3: Victim Descent Analysis by Income Areas
**Objective**: Analyze victim descent distribution in the 3 highest and  3 lowest income ZIP codes (2015)

**Results**:
```
Vict Descent   count
H              1,053    (Hispanic/Latin/Mexican)
W              610      (White)
B              349      (Black)
O              272      (Other)
X              71       (Unknown)
```

**Performance Analysis by Executor Count**:
| Executors | DataFrame API (Seconds) | SQL API (Seconds) | Total (Seconds) |
|-----------|-------------------|-------------|-----------|
| 4         | 20.41             | 11.69       | 69.70     |
| 3         | 18.57             | 16.14       | 57.10     |
| 2         | 19.14             | 14.76       | 54.86     |

**Key Insight**: 2 executors provided the best overall performance, suggesting optimal resource utilization for this query complexity


### Query 4: Police Department Response Analysis
**Objective**: Analyze whether crimes are handled by the nearest police department

#### 4a: Crimes Handled by Assigned Department
**Year-wise Analysis**:
| Year | Average Distance | Number of Crimes |
|------|------------------|------------------|
| 2010 | 0.0276           | 8,212            |
| 2011 | 0.0277           | 7,232            |
| 2012 | 0.0282           | 6,532            |
| 2013 | 0.0281           | 5,838            |
| 2014 | 0.0271           | 4,227            |
| 2015 | 0.0269           | 6,763            |
| 2016 | 0.0270           | 8,100            |
| 2017 | 0.0271           | 7,786            |
| 2018 | 0.0271           | 7,413            |
| 2019 | 0.0272           | 7,129            |
| 2020 | 0.0267           | 8,487            |
| 2021 | 0.0267           | 12,696           |
| 2022 | 0.0259           | 10,025           |
| 2023 | 0.0253           | 8,741            |

**Department-wise Analysis**:
| Division | Average Distance | Number of Crimes |
|----------|------------------|------------------|
| 77TH STREET | 0.027545450073381432 | 16,547 |
| SOUTHEAST | 0.02118506918085512 | 12,901 |
| NEWTON | 0.019467461882774557 | 9,608 |
| SOUTHWEST | 0.027964155549737753 | 8,633 |
| HOLLENBECK | 0.025364079062598754 | 6,111 |
| HARBOR | 0.03823754355960649 | 5,432 |
| RAMPART | 0.015490421719039171 | 4,989 |
| MISSION | 0.04432244810570649 | 4,459 |
| OLYMPIC | 0.017889611144352825 | 4,326 |
| NORTHEAST | 0.040094274284297966 | 3,846 |
| FOOTHILL | 0.03814789477256375 | 3,756 |
| NORTH HOLLYWOOD | 0.02624540049173441 | 3,642 |
| HOLLYWOOD | 0.0155868450287766 | 3,551 |
| CENTRAL | 0.011447910372430516 | 3,466 |
| WILSHIRE | 0.022800355180379914 | 3,420 |
| WEST VALLEY | 0.036295837282611061 | 2,786 |
| PACIFIC | 0.03740483574006487 | 2,743 |
| VAN NUYS | 0.02146496559565703 | 2,645 |
| DEVONSHIRE | 0.04089960561470661 | 2,501 |
| TOPANGA | 0.03294239027008409 | 2,310 |
| WEST LOS ANGELES | 0.04461626978171725 | 1,509 |

#### 4b: Crimes by Nearest Department
**Year-wise Closer Distance Analysis**:
| Year | Average Distance | Number of Crimes |
|------|------------------|------------------|
| 2010 | 0.02393765943798706 | 8,212 |
| 2011 | 0.02419068859979153 | 7,232 |
| 2012 | 0.024683058922273987 | 6,532 |
| 2013 | 0.02420117924330728 | 5,838 |
| 2014 | 0.02286932857278689 | 4,227 |
| 2015 | 0.023549220187993686 | 6,763 |
| 2016 | 0.02383136282115725 | 8,100 |
| 2017 | 0.023541801768831805 | 7,786 |
| 2018 | 0.023655510479937504 | 7,413 |
| 2019 | 0.023913869018817393 | 7,129 |
| 2020 | 0.023477185094103483 | 8,487 |
| 2021 | 0.023155050072480216 | 9,745 |
| 2022 | 0.022703655999980908 | 10,025 |
| 2023 | 0.02233296615917895 | 8,741 |


**Department-wise Analysis**:
| Division | Average Distance | Number of Crimes |
|----------|------------------|------------------|
| 77TH STREET | 0.016765062172506954 | 13,146 |
| SOUTHEAST | 0.02228952602037784 | 11,802 |
| SOUTHWEST | 0.022409814181910567 | 11,251 |
| NEWTON | 0.015270930651651138 | 7,057 |
| WILSHIRE | 0.024646800009519937 | 6,635 |
| HOLLENBECK | 0.02584837293528712 | 6,326 |
| OLYMPIC | 0.01692553925873392 | 5,644 |
| HARBOR | 0.036593769359882754 | 5,310 |
| HOLLYWOOD | 0.01972654225228524 | 5,083 |
| VAN NUYS | 0.029088788245648756 | 4,723 |
| RAMPART | 0.013422482936306584 | 4,633 |
| FOOTHILL | 0.03421348445624419 | 4,121 |
| NORTH HOLLYWOOD | 0.026761791055547907 | 3,721 |
| CENTRAL | 0.010269774712963668 | 3,624 |
| MISSION | 0.03622009603902812 | 3,036 |
| NORTHEAST | 0.03969321216682668 | 2,966 |
| WEST VALLEY | 0.026928721666376852 | 2,658 |
| PACIFIC | 0.03640195553537414 | 2,519 |
| TOPANGA | 0.029985904798642653 | 2,497 |
| DEVONSHIRE | 0.028852404442129054 | 1,283 |
| WEST LOS ANGELES | 0.03100979519676292 | 1,146 |


## ⚡ Performance Optimization

### Join Strategy Analysis
Tested four join strategies for optimal performance:

1. **BROADCAST**: Most efficient for large-small table combinations
2. **SORT MERGE**: Ideal for joining two large tables
3. **SHUFFLE_HASH**: Good for parallelizable keys with even distribution
4. **SHUFFLE_REPLICATE_NL**: Least efficient, computes Cartesian product

**Recommendation**: BROADCAST joins were most effective for our dataset characteristics

## 🚀 Installation & Execution

### Prerequisites
- Apache Hadoop 3.0+
- Apache Spark 3.4+
- Python 3.8+ with PySpark
- 2-node cluster setup

### Setup Instructions
```bash
# 1. Download data
chmod +x download_data.sh
./download_data.sh

# 2. Upload data to HDFS
hdfs dfs -put Crime_Data*.csv /user/user/

# 3. Run queries
spark-submit query1.py
spark-submit query2.py
spark-submit query3.py
spark-submit query4.py
```

### Configuration
- **HDFS Master**: okeanos-master:54310
- **Default Port**: 5000
- **Executors**: Configurable (2, 3, or 4)

## 📊 Key Findings & Insights

### Performance Conclusions
- **SQL API** generally outperformed DataFrame API for complex aggregations
- **DataFrame API** was faster for simpler transformations and filtering
- **RDD API** was significantly slower due to lack of Catalyst optimizer benefits
- **2 executors** provided optimal performance for most queries

### Crime Pattern Insights
- **77TH STREET** division handled the most firearm crimes (16,547 incidents)
- **Night hours** had the highest frequency of street crimes (688,121 incidents)
- **Hispanic/Latin** victims were most prevalent in high and low-income areas
- Crime response distances have **decreased over time**, suggesting improved efficiency
