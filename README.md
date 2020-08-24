# jmeter-docker

Docker files and test jmeter JMX file to start a jmeter cluster and run a standalone and distributed load test

## Tools Used
- apache-jmeter-5.3
- openjdk-8-jdk
- docker

## Build Image from Docker File

```bash
cd jmeter-base/
docker build --tag dinusha00/jmbase:latest .

cd jmeter-server/
docker build --tag dinusha00/jmserver:latest .

cd jmeter-master/
docker build --tag dinusha00/jmmaster:latest .
```

## Start Containers

### Start Slave Nodes
```bash
sudo docker run -dit --name slave01 dinusha00/jmserver /bin/bash

sudo docker run -dit --name slave02 dinusha00/jmserver /bin/bash

sudo docker run -dit --name slave03 dinusha00/jmserver /bin/bash

```

### Start Master Nodes
```bash
sudo docker run -dit --name master dinusha00/jmmaster /bin/bash

```


## Extract Slave Node IPs for Distributed Mode

```bash
sudo docker inspect --format '{{ .Name }} => {{ .NetworkSettings.IPAddress }}' $(sudo docker ps -a -q)

/slave03 => 172.17.0.5
/slave02 => 172.17.0.4
/slave01 => 172.17.0.3
/master => 172.17.0.2
```

## Login to the Master/Client Node to Run the Load Test
Use either of below options to go inside the node 
- SSH 
- Docker desktop CLI button

## Create the Test Jmeter Load Test Inside the Master/Client node
- Copy from jmeter-docker/sample-test/sample-test.jmx

## Run Standalone Mode

```bash
jmeter -n -t sample-test/sample-test.jmx -Jserver.rmi.ssl.disable=true

```

## Result of Standalone Mode

```bash
Aug 24, 2020 9:40:27 AM java.util.prefs.FileSystemPreferences$1 run
INFO: Created user preferences directory.
Creating summariser <summary>
Created the tree successfully using sample-test/sample-test.jmx
Starting standalone test @ Mon Aug 24 09:40:27 GMT 2020 (1598262027722)
Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
Warning: Nashorn engine is planned to be removed from a future JDK release
summary +      3 in 00:00:02 =    1.7/s Avg:   172 Min:   108 Max:   292 Err:     0 (0.00%) Active: 2 Started: 2 Finished: 0
summary +    327 in 00:00:28 =   11.6/s Avg:   110 Min:    99 Max:   257 Err:     0 (0.00%) Active: 0 Started: 5 Finished: 5
summary =    330 in 00:00:30 =   11.0/s Avg:   111 Min:    99 Max:   292 Err:     0 (0.00%)
Tidying up ...    @ Mon Aug 24 09:40:58 GMT 2020 (1598262058297)
... end of run

```

## Run Distributed Mode
```bash
jmeter -n -t sample-test/sample-test.jmx -R172.17.0.3,172.17.0.4,172.17.0.5 -Jserver.rmi.ssl.disable=true
```

## Result of Distributed Mode
```bash
Creating summariser <summary>
Created the tree successfully using sample-test/sample-test.jmx
Configuring remote engine: 172.17.0.3
Configuring remote engine: 172.17.0.4
Configuring remote engine: 172.17.0.5
Starting distributed test with remote engines: [172.17.0.4, 172.17.0.5, 172.17.0.3] @ Mon Aug 24 09:41:16 GMT 2020 (1598262076023)
Warning: Nashorn engine is planned to be removed from a future JDK release
Remote engines have been started:[172.17.0.4, 172.17.0.5, 172.17.0.3]
Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
summary =    996 in 00:00:31 =   32.5/s Avg:   110 Min:   100 Max:   339 Err:     0 (0.00%)
Tidying up remote @ Mon Aug 24 09:41:48 GMT 2020 (1598262108385)
... end of run
```

## Docker Hub References & Pre-Built Images
[dinusha00](https://hub.docker.com/)