# How can I deploy a basic Hadoop Cluster
## Target
Based on Docker, 3 containers for 4 nodes. One namenode and three datanode.  
main(namenode+datanode) + worker1(datanode) + worker2(datanode)
## Steps
### Framework
- The cluster will be managed by docker-compose file.
- There are already some settings for SSH in the framework. No need to handle
- There are main docker file, setup-main.sh and start-main.sh related to main node.
- There are worker docker file, setup-worker.sh and start-worker.sh related to worker node.
### Hadoop Installment
1. wget
2. Set HADOOP_HOME
3. Set User
### Hadoop Settings
1. Data path
2. Configs
    - core-site.xml: fs.DefaultFs will figure out which FS for Hadoop. Here we use "hdfs://main:9000" for all nodes.
    - hdfs-site.xml: 
    ```xml
    <configuration>
        <property>
            <name>dfs.namenode.name.dir</name>
            <value>file:///opt/hadoop/data/namenode</value>
        </property>
        <property>
            <name>dfs.datanode.data.dir</name>
            <value>file:///opt/hadoop/data/datanode</value>
        </property>
        <property>
            <name>dfs.replication</name>
            <value>3</value>
        </property>
    </configuration>
    ```
3. Get cluster together. Write workers' hostnames to the $HADOOP_HOME/etc/hadoop/workers. So they can be recognize when get started.
4. Start. First, to format the namenode. Only once is enough. The run start-dfs.sh to start all datanodes.

## Chores:
- Remind me of my settings of oh-my-zsh - history, EscEsc(sudo) and cat
- I used 5 tabs and they are in good order. deamon of Docker, Docker status, image building and composing, "main"'s bash, test
- Notice. If we restart from the previous containers, the datanodes on workers cannot be connected. So just remove them all with ``sudo docker rm $(docker ps -a -q)``
- Import commands:
## Important Commands
```bash
sudo docker rm $(sudo docker ps -a -q)
sudo docker-compose -f cs511p1-compose.yaml exec main bash
docker image ls # which is equal to docker imgages
sudo docker rmi -f $(sudo docker images -aq)
sudo docker-compose -f cs511p1-compose.yaml up
sudo docker run hello-world
dockerd 
echo "worker2" >> /opt/hadoop/etc/hadoop/workers
```

