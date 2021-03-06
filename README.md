#Yahoo! Cloud System Benchmark (YCSB)

This version of the YCSB tool adds adds support for Aerospike 3 to the Thumbtack Technology version that was originally modified to to add support for Aerospike and Couchbase databases, to improve MongoDB driver and to add some automation to run YCSB on multiple clients.


##Links
http://wiki.github.com/brianfrankcooper/YCSB/

https://github.com/couchbaselabs/YCSB

http://research.yahoo.com/Web_Information_Management/YCSB

https://fabric.readthedocs.org/en/1.3.2/


##Prerequisite

maven: http://maven.apache.org

pip: http://pip.readthedocs.org/en/latest/installing.html

##Step by step

###Download the latest release of YCSB
```
git clone https://github.com/aerospike/ycsb
cd ycsb    
mvn package
```
###Install the python Fabric module and time module
```
pip install fabric
pip install pytz
```
###Set up a database and client hosts to benchmark. 
There is a README file under each binding directory. You must have SSH (and in most cases root) access to all your hosts.
###Configure YCSB build script to build database binders.
Edit pom.xml, uncomment modules related to databases which you chose in `<modules>` section
###Configure hosts, databases and workloads settings
Edit files: conf/hosts.py, conf/databases.py, conf/workloads.py
###Build and deploy YCSB to client hosts
```
fab ycsb_deploy
```
###Load data to databases
```
fab ycsb_load:db=<dbname>
```
`<dbname>` is aerospike, couchbase, couchbase2, cassandra, mongodb or any other you configured.Edit conf/workloads.py to confiture workloads root directory
###Run YCSB workload
```
fab ycsb_run:db=<dbname>,workload=A
```
###Check the YCSB status
```
fab ycsb_status:db=<dbname>
```
###Download YCSB results and logs
```
fab ycsb_get:db=<dbname>,do=True
```    
You'll get some .out and .err files in the current directory downloaded from all your clients.
11. Aggregate the YCSB results
```
./bin/merge.py
```    
This script gets the most important parameters from YCSB .out files, such as throughput and latency, aggregates the results from multiple clients and prints the result as tab-separated values which can be easy pasted into any spreadsheet.

##Notes
This tool was tested using following software versions
* Ubuntu Server (12.04)
* Git (1.7.10.4)
* openjdk-7-jdk (7u9-2.3.3)
* Maven (2.2.1)
* Fabric (1.3.2)
* Python (2.7.3)
