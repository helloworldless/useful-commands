# Useful Commands

## git

| Command                                    | Description                                          | Example                                                         | Source                                                                                                           |
| ------------------------------------------ | ---------------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| git reset --hard HEAD^                     | Delete last local commit                             |                                                                 | https://sethrobertson.github.io/GitFixUm/fixup.html#remove_last                                                  |
| git log origin/master..HEAD                | Check local commits which aren't pushed              |                                                                 | https://stackoverflow.com/questions/2016901/viewing-unpushed-git-commits                                         |
| git diff origin/master..HEAD               | Diff local commits which aren't pushed               |                                                                 |                                                                                                                  |
| git diff <commit_id> `<remote>`/`<branch>` | Diff a commit to remote branch                       | git diff 0664e297fed39f65e58e9bfb2801377d21f9157a origin/master |                                                                                                                  |
| git diff <commit_id>^ <commit_id>          | Diff a commit to previous commit                     |                                                                 | https://stackoverflow.com/questions/17563726/how-to-see-the-changes-in-a-commit                                  |
| git log <commit_id> -`<number>`            | Log a limited number of previous changes of a commit | git log d08bcd673e1749bf1eb6f38f32684a712b7444fe -2             | https://git-scm.com/docs/git-log                                                                                 |
| git fetch origin                           |                                                      |                                                                 |
| git reset --hard origin/master             |                                                      |                                                                 |
| git clean                                  | Reset local branch to exactly match remote           |                                                                 | https://stackoverflow.com/questions/1628088/reset-local-repository-branch-to-be-just-like-remote-repository-head |
| git pull --rebase                          | Pull (fetch + rebase)                                |                                                                 |

## git clean
| Flag         | Description         |
| ------------ | ------------------- |
| -d           | Include directories |
| -n           | Dry run only        |
| -i           | Interactive mode    |
| -e <pattern> | Exclude a pattern   |

# Other

| Command                                                                                                                                                                                            | Alt                                                                                                                                                  | Comments       |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Expand-Archive '.\file.zip' '.\unziped\'                                                                                                                                                           |
| Get-Process -Id (Get-NetTCPConnection -LocalPort portNumber).OwningProcess                                                                                                                         | netstat -a -b -n -o                                                                                                                                  | find "redis"   | Needs refinement |
| taskkill /F /PID pid_number                                                                                                                                                                        |
| jar -xf .\redis-client.jar                                                                                                                                                                         |
| echo $Env:PATH                                                                                                                                                                                     | echo %PATH%                                                                                                                                          | dir %HOMEPATH% |
| mvn install:install-file -Dfile=`<path-to-file>`                                                                                                                                                   | mvn install:install-file -Dfile=`<path-to-file>` -DgroupId=`<group-id>` -DartifactId=`<artifact-id>` -Dversion=`<version>` -Dpackaging=`<packaging>` |
| cat for Windows: type file.txt                                                                                                                                                                     |
| kill $(ps aux &#124; grep 'java' &#124; awk '{print $2}')                                                                                                                                          |                                                                                                                                                      |
| Invoke-WebRequest -Uri "http://localhost:8080/task/add" -Body '{"first": 2, "second": 3}' -ContentType "application/json" -Method Post                                                             |
| curl http://localhost:8080/task/add -H "Content-Type: application/json" -d '{"first": 4, "second": 5}'                                                                                             |
| curl -X PUT http://localhost:8080/task/add -H "Content-Type: application/json" -d '{"first": 4, "second": 5}'                                                                                      |
| maven-local-install() { mvn install:install-file -Dfile=$PWD/target/$1-$2.jar -DgroupId=com.davidagood -DartifactId=$1 -Dversion=$2 -Dpackaging=jar  cp pom.xml ~/.m2/repository/$1/$2/$1-$2.pom } |


# AWS

|Description|Command|Alt|Refernece|Commments|
|-----------|-------|---|---------|---------|
||chmod 400 ~/Downloads/pizza-keys.pem||||
||ssh -i ~/Downloads/pizza-keys.pem ec2-user@<ip_address>||||
||scp -r -i ~/Downloads/pizza-keys.pem ./pizza-luvrs ec2-user@<ip_address>:/home/ec2-user/pizza-luvrs|||Remove destination folder name to overwrite existing contents at that location|
||sudo yum update||||
||curl -sL https://deb.nodesource.com/setup_12.x &#124; bash -|curl -sL https://deb.nodesource.com/setup_12.x &#124; sudo -E bash -|||
||sudo yum install -y nodejs||||
||sudo yum install -y nodejs||||
||ab -n 100 -c 5 http://pizza-load-balancer-1476197772.us-east-1.elb.amazonaws.com/||||

## CloudFormation

1. Deploy Example
    1. `aws cloudformation deploy --template-file ./hbfl.start.template --stack-name hbfl-stack --parameter-overrides ImageIdParameter=ami-0323c3dd2da7fb37d VPCIdParameter=vpc-0a358a64ae71dc0eb SubnetListParameter=subnet-0fd30ed5b383a78fb,subnet-043f91637234fc4bf --capabilities CAPABILITY_IAM`
    1. Note that re-running `aws cloudformat deploy` can be used for updates; It will use 
    all the previous parameters and you can add overrides as needed
1. Deploy Dry Run
    1. `aws cloudformation deploy ... --no-execute-changeset`
    1. The output of this command will include the command to inspec the changeset:
    `aws cloudformation describe-change-set --change-set-name <cloudformation_changeset_arn>`
1. Execute Changeset
    1. aws cloudformation execute-change-set --change-set-name <cloudformation_changeset_arn>

## Elastic Beanstalk

1. `brew install awsebcli`
1. `eb init`
1. `eb create`
1. `eb updated`

# More

## Basic script for auto-scaling group

```bash
#!/bin/bash
cd /home/ec2-user/pizza-luvrs
echo "starting pizza luvrs"
npm start
```


# Kafka

If installed with brew on MacOS, qualify commands with `/usr/local/bin/`, e.g. `/usr/local/bin/kafka-topics`

1. Start Zookeeper
    1. `zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties`
1. Start Kafka
    1. `kafka-server-start /usr/local/etc/kafka/server.properties`
1. Test Zookeeper connection
    1. `telnet localhost 2181`
    1. `nc -vz localhost 2181`
1. Create Topic
    1. `kafka-topics --create --topic my_topic --zookeeper localhost:2181 --replication-factor 1 --partitions 1`
1. Logs Location: `/usr/local/var/lib/kafka-logs/`
1. List Topics: `kafka-topics --list --zookeeper localhost:2181`
1. Produce a message from command line: `kafka-console-producer --broker-list localhost:9092 --topic my_topic`
1. Consumer messages from command line: `kafka-console-consumer --bootstrap-server localhost:9092 --topic my_topic --from-beginning`
1. Get topic details (e.g. partition count, replication factor, leader, replicas): `kafka-topics --describe --topic my_topic --zookeeper localhost:2181`
1. Running multiple brokers on one machine:
    1. https://www.michael-noll.com/blog/2013/03/13/running-a-multi-broker-apache-kafka-cluster-on-a-single-node/
    1. In summary, create copies of the `server.properties` file and give unique values to
        1. `broker.id`
        1. `listeners`
        1. `log.dirs`
    1. Then run `kafka-server-start` pointing to the modified `server.properties` file

# Misc

1. Delete node_modules recursively on Mac: `find . -name 'node_modules' -type d -prune -print -exec rm -rf '{}' \;`
