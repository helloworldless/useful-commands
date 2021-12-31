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
||curl -sL https://deb.nodesource.com/setup_12.x &#124; bash -|curl -sL https://rpm.nodesource.com/setup_12.x &#124; sudo -E bash -|||
||sudo yum install -y nodejs||||
||ab -n 100 -c 5 http://pizza-load-balancer-1476197772.us-east-1.elb.amazonaws.com/||||

## DynamoDB
    
### Create Basic Table (Single Table Design)
    
```shell
aws dynamodb create-table --table-name MyTable --attribute-definitions AttributeName=PK,AttributeType=S AttributeName=SK,AttributeType=S --key-schema AttributeName=PK,KeyType=HASH AttributeName=SK,KeyType=RANGE --billing-mode PAY_PER_REQUEST --endpoint-url http://localhost:4566
```

With Global Secondary Index (GSI):
    
```
aws dynamodb create-table --table-name MyTable --attribute-definitions AttributeName=PK,AttributeType=S AttributeName=SK,AttributeType=S AttributeName=GSI1PK,AttributeType=S AttributeName=GSI1SK,AttributeType=S --key-schema AttributeName=PK,KeyType=HASH AttributeName=SK,KeyType=RANGE --global-secondary-indexes 'IndexName=GSI1,KeySchema=[{AttributeName=GSI1PK,KeyType=HASH},{AttributeName=GSI1SK,KeyType=RANGE}],Projection={ProjectionType=ALL}' --billing-mode PAY_PER_REQUEST --endpoint-url http://localhost:4566
```
    
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

## IAM

### Assume Role Policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": [
                "<role_arn>"
            ]
        }
    ]
}
```

### Improving AWS Force MFA Policy for IAM Users

https://www.trek10.com/blog/improving-the-aws-force-mfa-policy-for-iam-users

### AWS CLI Config

~/.aws/config
```text
[default]
region = us-east-1
output = json

[profile local]
source_profile=default
dynamodb_endpoint_url=http://localhost:8000

[profile sso-stg]
sso_start_url = https://d-<account-id>.awsapps.com/start
sso_region = us-east-1
sso_account_id = <sso-account-id>
sso_role_name = <role-name>
region = us-east-1
output = json
    
[profile microtrader-admin]
source_profile = microtrader
role_arn = <arn_of_role_to_assume>
region = us-east-1
mfa_serial = <iam_user_mfa_device_arn>
```

~/.aws/credentials
```text
[default]
aws_access_key_id = <id>
aws_secret_access_key = <key>
    
[local]
aws_access_key_id = dummy
aws_secret_access_key = dummy

[microtrader]
aws_access_key_id = <id>
aws_secret_access_key = <key>
```

Then to use the profile, `export AWS_PROFILE=microtrader-admin`. When you run a command 
it should prompt you for an MFA token.

## ECS and ECR

### Connect Docker Client to ECR and Pusing an Image and Tag

1. `aws ecr get-login --no-include-email --region us-east-1`
1. Run the resulting command or just run it in an `eval $()` 
1. `docker build -t dockerproductionaws/microtrader-base .`
1. `docker tag dockerproductionaws/microtrader-base:latest <aws_ecr_url>/dockerproductionaws/microtrader-base:latest`
1. `docker push <aws_ecr_url>/dockerproductionaws/microtrader-base:latest`

### ECS Agent Debugging

1. ECS agent log location: `var/log/ecs`
1. ECS agent introspection endpoint: `curl -s localhost:51678/v1/metadata | jq`
1. ECS agent tasks endpoint: `curl -s localhost:51678/v1/tasks | jq`

### Make EC2 Join ECS Cluster at Startup
`echo ECS_CLUSTER=cluster_name_here > /etc/ecs/ecs.config`

## More

## Basic script for auto-scaling group

```bash
#!/bin/bash
cd /home/ec2-user/pizza-luvrs
echo "starting pizza luvrs"
npm start
```

# Docker

## Inspect Container Host Bind Mounts

`docker inspect -f '{{json .HostConfig.Binds}}' ecs-agent | jq`

## Postgres Docker

See notes about setting environment variables [here](https://github.com/docker-library/docs/blob/master/postgres/README.md#environment-variables).

Note that defaults are used when running on `localhost`. Override a default in the `run` command like this: `-e POSTGRES_PASSWORD=secret`

1. `docker run -d --name <name> -v <volume_name>:/var/lib/postgresql/data -p 5432:5432 postgres:latest`
    1. Or with env overrides: `docker run -d --name <name> -e POSTGRES_USER=<user> -e POSTGRES_PASSWORD=<password> -e POSTGRES_DB=<database_name> -v <volume_name>:/var/lib/postgresql/data -p 5432:5432 postgres:latest`
1. `docker logs -f <name>`
1. `docker exec -it <name> psql -U postgres`
    1. `\l` to list databases
    1. `CREATE DATABASE test;`
    1. `\c test` to change to test database
    1. `INSERT INTO relation(id, parentId, childId) VALUES (2, 2, 3);`
    1. `INSERT INTO relation(id, parentId, childId, createdAt) VALUES (1, 1, 2, timezone('UTC'::text, now()));`
    1. `SELECT * FROM relation;`

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

## Delete node_modules recursively on Mac

`find . -name 'node_modules' -type d -prune -print -exec rm -rf '{}' \;`

## Proxy A Command And Run Arbitrary Commands First 

```bash
#!/bin/bash
set -e

# Add initialisation logic here

# Run application
exec "$@"
```

## Zip Specific Files/Directories in Windows (PowerShell)

`Compress-Archive -LiteralPath node_modules, index.js -DestinationPath yourfilename.zip`


# Gralde

1. Upgrade Gradle wrapper: `./gradlew wrapper --gradle-version 6.6.1`
    1. Source: https://docs.gradle.org/current/userguide/gradle_wrapper.html#sec:upgrading_wrapper

## Useful for Troubleshooting

1. Print Java Source Files
    1. `println "sourceSets.main.allSource.files: " + sourceSets.main.allSource.files`
    1. Example output: `sourceSets.main.allSource.files: [/home/dgood/IdeaProjects/cdk-java-starter/lambda/src/main/resources/a.txt, /home/dgood/IdeaProjects/cdk-java-starter/lambda/src/main/java/com/davidagood/cdkjava/ExampleHandler.java]`
1. Print all configurations
    1. `println "configurations: " + configurations`
    1. Example output: `configurations: [configuration ':lambda:annotationProcessor', configuration ':lambda:apiElements', configuration ':lambda:archives', configuration ':lambda:compile', configuration ':lambda:compileClasspath', configuration ':lambda:compileOnly', configuration ':lambda:default', configuration ':lambda:implementation', configuration ':lambda:lambdaResources', configuration ':lambda:outputAnnotationProcessor', configuration ':lambda:outputCompile', configuration ':lambda:outputCompileClasspath', configuration ':lambda:outputCompileOnly', configuration ':lambda:outputImplementation', configuration ':lambda:outputRuntime', configuration ':lambda:outputRuntimeClasspath', configuration ':lambda:outputRuntimeOnly', configuration ':lambda:runtime', configuration ':lambda:runtimeClasspath', configuration ':lambda:runtimeElements', configuration ':lambda:runtimeOnly', configuration ':lambda:testAnnotationProcessor', configuration ':lambda:testCompile', configuration ':lambda:testCompileClasspath', configuration ':lambda:testCompileOnly', configuration ':lambda:testImplementation', configuration ':lambda:testRuntime', configuration ':lambda:testRuntimeClasspath', configuration ':lambda:testRuntimeOnly']`
    
