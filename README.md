<!--
Copyright (c) 2010 Yahoo! Inc., 2012 - 2016 YCSB contributors.
All rights reserved.

Licensed under the Apache License, Version 2.0 (the "License"); you
may not use this file except in compliance with the License. You
may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
implied. See the License for the specific language governing
permissions and limitations under the License. See accompanying
LICENSE file.
-->

Yahoo! Cloud System Benchmark (YCSB)
====================================
[![Build Status](https://travis-ci.org/brianfrankcooper/YCSB.png?branch=master)](https://travis-ci.org/brianfrankcooper/YCSB)


### Custom Hadoop-2.7.2-MEDEA version with HBASE-1.2.3
**Medea** compatibility
Build Command:

    mvn -pl com.yahoo.ycsb:hbase10-binding -am clean package

Configure:
    
    ./deploy.sh -slider-hbase-client # From deployment machine
    ./yarn-dev-panos/hbase_client/bin/hbase shell
    n_splits = 200 # HBase recommends (10 * number of regionservers)
    create 'usertable', 'family', {SPLITS => (1..n_splits).map {|i| "user#{1000+i*(9999-1000)/n_splits}"}}

Run Workload:
    
    bin/ycsb load hbase10 -P workloads/workloada -cp /HBASE-HOME-DIR/conf -p table=usertable -p columnfamily=family -s
    bin/ycsb run hbase10 -p table=usertable -p columnfamily=family -threads 256 -P workloads/workloada -p measurementtype=raw -s -p measurement.raw.output_file=./results/yarn/write-wA-1R.dat > ./results/yarn/write-wA-1R-sum.dat ; \
    bin/ycsb run hbase10 -p table=usertable -p columnfamily=family -threads 256 -P workloads/workloadb -p measurementtype=raw -s -p measurement.raw.output_file=./results/yarn/write-wB-1R.dat > ./results/yarn/write-wB-1R-sum.dat ; \
    bin/ycsb run hbase10 -p table=usertable -p columnfamily=family -threads 256 -P workloads/workloada -p measurementtype=raw -s -p measurement.raw.output_file=./results/yarn/write-wC-1R.dat > ./results/yarn/write-wC-1R-sum.dat ; \
    bin/ycsb run hbase10 -p table=usertable -p columnfamily=family -threads 256 -P workloads/workloada -p measurementtype=raw -s -p measurement.raw.output_file=./results/yarn/write-wD-1R.dat > ./results/yarn/write-wD-1R-sum.dat ; \
    bin/ycsb run hbase10 -p table=usertable -p columnfamily=family -threads 256 -P workloads/workloada -p measurementtype=raw -s -p measurement.raw.output_file=./results/yarn/write-wE-1R.dat > ./results/yarn/write-wE-1R-sum.dat ; \ 
    bin/ycsb run hbase10 -p table=usertable -p columnfamily=family -threads 256 -P workloads/workloada -p measurementtype=raw -s -p measurement.raw.output_file=./results/yarn/write-wF-1R.dat > ./results/yarn/write-wF-1R-sum.dat


Links
-----
http://wiki.github.com/brianfrankcooper/YCSB/  
https://labs.yahoo.com/news/yahoo-cloud-serving-benchmark/
ycsb-users@yahoogroups.com  

Getting Started
---------------

1. Download the [latest release of YCSB](https://github.com/brianfrankcooper/YCSB/releases/latest):

    ```sh
    curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.11.0/ycsb-0.11.0.tar.gz
    tar xfvz ycsb-0.11.0.tar.gz
    cd ycsb-0.11.0
    ```
    
2. Set up a database to benchmark. There is a README file under each binding 
   directory.

3. Run YCSB command. 

    On Linux:
    ```sh
    bin/ycsb.sh load basic -P workloads/workloada
    bin/ycsb.sh run basic -P workloads/workloada
    ```

    On Windows:
    ```bat
    bin/ycsb.bat load basic -P workloads\workloada
    bin/ycsb.bat run basic -P workloads\workloada
    ```

  Running the `ycsb` command without any argument will print the usage. 
   
  See https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload
  for a detailed documentation on how to run a workload.

  See https://github.com/brianfrankcooper/YCSB/wiki/Core-Properties for 
  the list of available workload properties.

Building from source
--------------------

YCSB requires the use of Maven 3; if you use Maven 2, you may see [errors
such as these](https://github.com/brianfrankcooper/YCSB/issues/406).

To build the full distribution, with all database bindings:

    mvn clean package

To build a single database binding:

    mvn -pl com.yahoo.ycsb:mongodb-binding -am clean package
