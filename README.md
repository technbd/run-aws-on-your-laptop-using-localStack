# Run AWS on Your Laptop using LocalStack and Testing Terraform code locally for AWS:

LocalStack is a cloud service emulator that runs in a single container on your laptop or in your CI environment. With LocalStack, you can run your AWS applications or Lambdas entirely on your local machine without connecting to a remote cloud provider. It is an open-source framework designed to emulate cloud environments locally for development and testing purposes. 

__Some key features of LocalStack:__

- **Local Cloud Services:** It provides a local environment that simulates various cloud services like S3 (Simple Storage Service), DynamoDB, Lambda, SQS (Simple Queue Service), SNS (Simple Notification Service), and more.

- **Easy Integration:** Developers can integrate LocalStack into their workflow effortlessly. It offers a command-line interface (CLI) and supports various programming languages and platforms.

- **Offline Development:** Enables developers to work offline by simulating cloud services locally, reducing dependencies on actual cloud infrastructure during development and testing.

- **Cost-Efficiency:** By testing and developing locally, it can potentially reduce costs associated with cloud usage during development stages.

- **Customization and Extensibility:** It allows customization and extension of services, enabling developers to tailor the environment to match specific project requirements.


__Here are some specific groups that can benefit from LocalStack:__

- A beginner who is looking to practice your AWS skills but don't have access to credit card which is required upon AWS registration.
- A student, who wants to gain hands-on experience with AWS services without spending any money.
- A professional who wants to troubleshoot or test your infrastructure configurations offline on your machine, without setting up a separate cloud environment for testing, and then seamlessly transition to your main AWS production environment once everything is ready and optimized,
then LocalStack has got you covered. 
- Teams focusing on automation, deployment, and infrastructure management use LocalStack to simulate cloud environments for local development and testing of deployment pipelines.
- Quality assurance professionals use LocalStack to create controlled environments for testing, ensuring the application works seamlessly with cloud services before deployment.
- Small or Startups Companies with limited resources use LocalStack to reduce costs associated with development and testing by simulating cloud environments locally.


### LocalStack is following core Cloud APIs test on your local machine:

There are currently more than 60 emulated AWS cloud services (and most of them free) provided by LocalStack. As an introduction we will create an S3 bucket and emulate the deployment of a static website and witness that it works just as if it were deployed on AWS S3.

- ACM , API Gateway, CloudFormation, CloudWatch
- CloudWatch Logs, DynamoDB, DynamoDB Streams
- EC2, Elasticsearch Service, EventBridge (CloudWatch Events)
- Firehose, IAM, Kinesis, KMS, Lambda, Redshift
- Route53, S3, SecretsManager, SES, SNS
- SQS, SSM, StepFunctions, STS


### Prerequisites:
To setup and use LocalStack effectively, there are several prerequisites you'll need to have in place:

- AWS CLI
- Python3 (3.8 up to 3.11 supported)
- PIP (package installer for Python)
- Docker and Docker compose (v1.9.0+)
- OpenSSL 1.1.1+


### Install AWS CLI in Linux and Check Prerequisites Version: 

```
### Install AWS CLI:

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip
./aws/install


### AWS CLI Version Check:

aws --version

output:
------
aws-cli/2.13.32 Python/3.11.6 Linux/3.10.0-1160.83.1.el7.x86_64 exe/x86_64.centos.7 prompt/off
```


```
### Python Version Check:

python3.8 -V
    Python 3.8.5
```


```
### PIP Version Check:

pip3.8 -V
    pip 23.3.1 from /python/python-3.8.5/py-data/lib/python3.8/site-packages/pip (python 3.8)

```


```
### Docker Version Check:

docker --version
    Docker version 23.0.1, build a5ee5b1


### Check Docker Compose Version:

docker-compose -v
    docker-compose version 1.29.2, build 5becea4c
```


```
### OpenSSL Version check:

openssl version
    OpenSSL 1.1.1w  11 Sep 2023 (Library: OpenSSL 1.1.1k  FIPS 25 Mar 2021)

```


# Part-1:

### Install LocalStack:
Installing LocalStack can be done through various methods, each suited to different preferences and use cases.  You can install it using the python package manager (pip) or use docker run/docker compose a LocalStack container. 


<details>
  <summary>LocalStack RUN Using PIP (Method-1):</summary>

```
### Install python virtual environmnet pacakge:

pip3.8 install --upgrade pip

pip3.8 install virtualenv

```


```
### Check Module list:

pip3.8 list

Package      Version
------------ -------
pip          23.3.1
virtualenv   20.24.6

```

### Install LocalStack CLI package in the python environment directory: 
Note: Do not use "sudo or root" user - LocalStack should be installed and started entirely under a local non-root user and proper directory permission:

```
su - <user_name>

mkdir -p localstack/project1
cd localstack/project1/

python3.8 -m venv test_venv
source test_venv/bin/activate

```


```
### Update PIP under the project directory:

python3.8 -m pip install --upgrade pip

Or,

pip3.8 install --upgrade pip
```


```
### Install LocalStack package under virtual directory:

python3.8 -m pip install localstack

Or,

pip3.8 install localstack
```


```
localstack --version
    3.0.1
```


```
### If any error like "ImportError: urllib3 v2 only supports OpenSSL 1.1.1+, currently the 'ssl' module is compiled with 'OpenSSL 1.0.2k-fips  26 Jan 2017'. See: https://github.com/urllib3/urllib3/issues/2168" 

then run this command: 

pip3.8 install urllib3==1.26.6

```


### Start LocalStack inside a Docker container: 

The next step is to run LocalStack. Before you start run localstack, ensure that Docker service is up & running. This will start LocalStack on localhost port 4566. So open a terminal and run the following:

```
### Normal user add to "docker" group:

echo $USER
sudo usermod -aG docker $USER

sudo chown -R $USER:$USER /var/run/docker.sock
```

```
### For Command Help:

localstack -h


### Start localstack in docker mode, from a container:

localstack start -d


     __                     _______ __             __
    / /   ____  _________ _/ / ___// /_____ ______/ /__
   / /   / __ \/ ___/ __ `/ /\__ \/ __/ __ `/ ___/ //_/
  / /___/ /_/ / /__/ /_/ / /___/ / /_/ /_/ / /__/ ,<
 /_____/\____/\___/\__,_/_//____/\__/\__,_/\___/_/|_|

 💻 LocalStack CLI 3.0.1

[05:34:49] starting LocalStack in Docker mode 🐳         localstack.py:495
           preparing environment                         bootstrap.py:1241
           configuring container                         bootstrap.py:1249
[05:37:20] starting container                            bootstrap.py:1259
[05:37:23] detaching                                     bootstrap.py:1263

```


```
### Check docker container status: 

docker ps

CONTAINER ID   IMAGE                   COMMAND                  CREATED     STATUS                   PORTS                                          NAMES
c1bc4b8bd4ed   localstack/localstack   "docker-entrypoint.sh"   4 minutes   Up 4 minutes (healthy)   127.0.0.1:4510-4560->4510-4560/tcp, 0.0.0.0:53->53/tcp, 0.0.0.0:53->53/udp, 127.0.0.1:4566->4566/tcp, 5678/tcp   localstack-main
```


```
localstack logs
```


```
### To check the various services that it provides by Localstack:

localstack status services

┏━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━┓
┃ Service                  ┃ Status      ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━┩
│ acm                      │ ✔ available │
│ apigateway               │ ✔ available │
│ cloudformation           │ ✔ available │
│ cloudwatch               │ ✔ available │
│ config                   │ ✔ available │
│ dynamodb                 │ ✔ available │
│ dynamodbstreams          │ ✔ available │
│ ec2                      │ ✔ available │
│ es                       │ ✔ available │
│ events                   │ ✔ available │
│ firehose                 │ ✔ available │
│ iam                      │ ✔ available │
│ kinesis                  │ ✔ available │
│ kms                      │ ✔ available │
│ lambda                   │ ✔ available │
│ logs                     │ ✔ available │
│ opensearch               │ ✔ available │
│ redshift                 │ ✔ available │
│ resource-groups          │ ✔ available │
│ resourcegroupstaggingapi │ ✔ available │
│ route53                  │ ✔ available │
│ route53resolver          │ ✔ available │
│ s3                       │ ✔ available │
│ s3control                │ ✔ available │
│ scheduler                │ ✔ available │
│ secretsmanager           │ ✔ available │
│ ses                      │ ✔ available │
│ sns                      │ ✔ available │
│ sqs                      │ ✔ available │
│ ssm                      │ ✔ available │
│ stepfunctions            │ ✔ available │
│ sts                      │ ✔ available │
│ support                  │ ✔ available │
│ swf                      │ ✔ available │
│ transcribe               │ ✔ available │
└──────────────────────────┴─────────────┘

```


```
### Check the Localstack configuration details:

localstack config show

┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ Key                                          ┃ Value                                    ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┩
│ ALLOW_NONSTANDARD_REGIONS                    │ False                                    │
│ BOTO_WAITER_DELAY                            │ 1                                        │
│ BOTO_WAITER_MAX_ATTEMPTS                     │ 120                                      │
│ BUCKET_MARKER_LOCAL                          │ hot-reload                               │
│ CFN_IGNORE_UNSUPPORTED_RESOURCE_TYPES        │ True                                     │
│ CFN_RESOURCE_PROVIDER_OVERRIDES              │ {}                                       │
│ CFN_VERBOSE_ERRORS                           │ False                                    │
│ CUSTOM_SSL_CERT_PATH                         │                                          │
│ DATA_DIR                                     │                                          │
│ DEBUG                                        │ False                                    │
│ DEBUG_HANDLER_CHAIN                          │ False                                    │
│ DEVELOP                                      │ False                                    │
│ DEVELOP_PORT                                 │ 5678                                     │
│ DISABLE_BOTO_RETRIES                         │ False                                    │
│ DISABLE_CORS_CHECKS                          │ False                                    │
│ DISABLE_CORS_HEADERS                         │ False                                    │
│ DISABLE_CUSTOM_BOTO_WAITER_CONFIG            │ False                                    │
│ DISABLE_CUSTOM_CORS_APIGATEWAY               │ False                                    │
│ DISABLE_CUSTOM_CORS_S3                       │ False                                    │
│ DISABLE_EVENTS                               │ False                                    │
│ DNS_ADDRESS                                  │ 0.0.0.0                                  │
│ DNS_LOCAL_NAME_PATTERNS                      │                                          │
│ DNS_PORT                                     │ 53                                       │
│ DNS_RESOLVE_IP                               │ 127.0.0.1                                │
│ DNS_SERVER                                   │ None                                     │
│ DNS_VERIFICATION_DOMAIN                      │ localstack.cloud                         │
│ DOCKER_BRIDGE_IP                             │ 172.17.0.1                               │
│ DOCKER_SDK_DEFAULT_TIMEOUT_SECONDS           │ 60                                       │
│ DYNAMODB_ERROR_PROBABILITY                   │ 0.0                                      │
│ DYNAMODB_HEAP_SIZE                           │ 256m                                     │
│ DYNAMODB_LOCAL_PORT                          │ 0                                        │
│ DYNAMODB_READ_ERROR_PROBABILITY              │ 0.0                                      │
│ DYNAMODB_SHARE_DB                            │ 0                                        │
│ DYNAMODB_WRITE_ERROR_PROBABILITY             │ 0.0                                      │
│ EAGER_SERVICE_LOADING                        │ False                                    │
│ ENABLE_CONFIG_UPDATES                        │ False                                    │
│ EXTRA_CORS_ALLOWED_HEADERS                   │                                          │
│ EXTRA_CORS_ALLOWED_ORIGINS                   │                                          │
│ EXTRA_CORS_EXPOSE_HEADERS                    │                                          │
│ GATEWAY_LISTEN                               │ [HostAndPort(host=127.0.0.1, port=4566)] │
│ HOSTNAME_FROM_LAMBDA                         │                                          │
│ KINESIS_ERROR_PROBABILITY                    │ 0.0                                      │
│ KINESIS_MOCK_LOG_LEVEL                       │                                          │
│ KINESIS_MOCK_PERSIST_INTERVAL                │ 5s                                       │
│ KINESIS_ON_DEMAND_STREAM_COUNT_LIMIT         │ 10                                       │
│ LAMBDA_DOCKER_DNS                            │                                          │
│ LAMBDA_DOCKER_FLAGS                          │                                          │
│ LAMBDA_DOCKER_NETWORK                        │                                          │
│ LAMBDA_INIT_BIN_PATH                         │ None                                     │
│ LAMBDA_INIT_BOOTSTRAP_PATH                   │ None                                     │
│ LAMBDA_INIT_DEBUG                            │ False                                    │
│ LAMBDA_INIT_DELVE_PATH                       │ None                                     │
│ LAMBDA_INIT_DELVE_PORT                       │ 40000                                    │
│ LAMBDA_INIT_POST_INVOKE_WAIT_MS              │ None                                     │
│ LAMBDA_INIT_RELEASE_VERSION                  │ None                                     │
│ LAMBDA_INIT_USER                             │ None                                     │
│ LAMBDA_JAVA_OPTS                             │                                          │
│ LAMBDA_KEEPALIVE_MS                          │ 600000                                   │
│ LAMBDA_LIMITS_CODE_SIZE_UNZIPPED             │ 262144000                                │
│ LAMBDA_LIMITS_CODE_SIZE_ZIPPED               │ 52428800                                 │
│ LAMBDA_LIMITS_CONCURRENT_EXECUTIONS          │ 1000                                     │
│ LAMBDA_LIMITS_CREATE_FUNCTION_REQUEST_SIZE   │ 70167211                                 │
│ LAMBDA_LIMITS_MAX_FUNCTION_ENVVAR_SIZE_BYTES │ 4096                                     │
│ LAMBDA_LIMITS_MINIMUM_UNRESERVED_CONCURRENCY │ 100                                      │
│ LAMBDA_LIMITS_TOTAL_CODE_SIZE                │ 80530636800                              │
│ LAMBDA_REMOVE_CONTAINERS                     │ True                                     │
│ LAMBDA_RETRY_BASE_DELAY_SECONDS              │ 60                                       │
│ LAMBDA_RUNTIME_ENVIRONMENT_TIMEOUT           │ 10                                       │
│ LAMBDA_RUNTIME_EXECUTOR                      │                                          │
│ LAMBDA_RUNTIME_IMAGE_MAPPING                 │                                          │
│ LAMBDA_SYNCHRONOUS_CREATE                    │ False                                    │
│ LAMBDA_TRUNCATE_STDOUT                       │ 2000                                     │
│ LEGACY_DOCKER_CLIENT                         │ False                                    │
│ LEGACY_SNS_GCM_PUBLISHING                    │ False                                    │
│ LOCALSTACK_HOST                              │ localhost.localstack.cloud:4566          │
│ LS_LOG                                       │ False                                    │
│ MAIN_CONTAINER_NAME                          │ localstack-main                          │
│ MAIN_DOCKER_NETWORK                          │                                          │
│ OPENSEARCH_ENDPOINT_STRATEGY                 │ domain                                   │
│ OUTBOUND_HTTPS_PROXY                         │                                          │
│ OUTBOUND_HTTP_PROXY                          │                                          │
│ PARITY_AWS_ACCESS_KEY_ID                     │ False                                    │
│ PERSISTENCE                                  │ False                                    │
│ PORTS_CHECK_DOCKER_IMAGE                     │                                          │
│ S3_SKIP_KMS_KEY_VALIDATION                   │ True                                     │
│ S3_SKIP_SIGNATURE_VALIDATION                 │ True                                     │
│ SKIP_INFRA_DOWNLOADS                         │                                          │
│ SKIP_SSL_CERT_DOWNLOAD                       │ False                                    │
│ SNAPSHOT_FLUSH_INTERVAL                      │ 15                                       │
│ SNAPSHOT_LOAD_STRATEGY                       │                                          │
│ SNAPSHOT_SAVE_STRATEGY                       │                                          │
│ SQS_CLOUDWATCH_METRICS_REPORT_INTERVAL       │ 60                                       │
│ SQS_DELAY_PURGE_RETRY                        │ False                                    │
│ SQS_DELAY_RECENTLY_DELETED                   │ False                                    │
│ SQS_DISABLE_CLOUDWATCH_METRICS               │ False                                    │
│ SQS_ENDPOINT_STRATEGY                        │ standard                                 │
│ STEPFUNCTIONS_LAMBDA_ENDPOINT                │                                          │
│ STRICT_SERVICE_LOADING                       │ True                                     │
│ TF_COMPAT_MODE                               │ False                                    │
│ USE_SSL                                      │ False                                    │
│ WAIT_FOR_DEBUGGER                            │ False                                    │
│ WINDOWS_DOCKER_MOUNT_PREFIX                  │ /host_mnt                                │
└──────────────────────────────────────────────┴──────────────────────────────────────────┘

```


```
### Now health check in the Linux curl command: 

curl http://localhost:4566/_localstack/health


{"services": {"acm": "available", "apigateway": "available", "cloudformation": "available", "cloudwatch": "available", "config": "available", "dynamodb": "available", "dynamodbstreams": "available", "ec2": "available", "es": "available", "events": "available", "firehose": "available", "iam": "available", "kinesis": "available", "kms": "available", "lambda": "available", "logs": "available", "opensearch": "available", "redshift": "available", "resource-groups": "available", "resourcegroupstaggingapi": "available", "route53": "available", "route53resolver": "available", "s3": "available", "s3control": "available", "scheduler": "available", "secretsmanager": "available", "ses": "available", "sns": "available", "sqs": "available", "ssm": "available", "stepfunctions": "available", "sts": "available", "support": "available", "swf": "available", "transcribe": "available"}, "edition": "community", "version": "3.0.1.dev"}

```


```
### Check localstack basic info:

curl http://localhost:4566/_localstack/info

{"version": "3.0.1.dev:61d66a97", "edition": "community", "is_license_activated": false, "session_id": "d8d0c768-5b2d-4b57-a277-87a1213578e5", "machine_id": "gen_19d960eb-0e3", "system": "docker", "is_docker": true, "server_time_utc": "2023-11-25T19:28:20", "uptime": 831}

```


### Configuring Localstack for Remote access: 
We see the above GATEWAY_LISTEN configuration option is set to 127.0.0.1 on port 4566. This configuration option makes localstack available only on localhost, but not accessible over any network IP address. In order to make it accessible over the network, you must make localstack available to all of its network IP addresses. We need to change the default configuration as **GATEWAY_LISTEN 127.0.0.1:4566** to **GATEWAY_LISTEN 0.0.0.0:4566**.


```
### Create "network.env" file:

mkdir ~/.localstack
touch ~/.localstack/network.env
echo "GATEWAY_LISTEN=0.0.0.0:4566" > ~/.localstack/network.env


cat ~/.localstack/network.env

	GATEWAY_LISTEN=0.0.0.0:4566

```


```
tree ~/.localstack
/home/<username>/.localstack
└── network.env

```


```
### Stop the previous container:

localstack stop


### Again, Starting the localstack by passing the "--profile" as a command line option:

localstack --profile=network start -d 


     __                     _______ __             __
    / /   ____  _________ _/ / ___// /_____ ______/ /__
   / /   / __ \/ ___/ __ `/ /\__ \/ __/ __ `/ ___/ //_/
  / /___/ /_/ / /__/ /_/ / /___/ / /_/ /_/ / /__/ ,<
 /_____/\____/\___/\__,_/_//____/\__/\__,_/\___/_/|_|

 💻 LocalStack CLI 3.0.1
 👤 Profile: network

[22:47:38] starting LocalStack in Docker mode 🐳         localstack.py:495
           preparing environment                         bootstrap.py:1241
           configuring container                         bootstrap.py:1249
[22:47:39] starting container                            bootstrap.py:1259
[22:47:41] detaching                                     bootstrap.py:1263

```


```
docker ps

CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS                   PORTS                                          NAMES
c3f0dfbdfbb9   localstack/localstack   "docker-entrypoint.sh"   3 seconds ago   Up 2 seconds (healthy)   0.0.0.0:53->53/tcp, 0.0.0.0:4510-4560->4510-4560/tcp, 0.0.0.0:4566->4566/tcp, 0.0.0.0:53->53/udp, 5678/tcp   localstack-main

```



</details>





<details>
  <summary>LocalStack RUN Using Docker (Method-2):</summary>

Here's an example of a "docker-compose.yaml" file that sets up LocalStack using Docker Compose:

```
vim docker-compose.yaml

version: "3"
services:
  localstack:
    image: localstack/localstack:latest
    container_name: localstack
    
    ports:
      - "4566:4566"            # LocalStack Gateway
      - "4510-4559:4510-4559"  # external services port range
    
    environment:
      - DEBUG=${DEBUG-}     	# show all logs
      - DOCKER_HOST=unix:///var/run/docker.sock
      #- SERVICES=s3, iam, lambda, dynamodb, cloudwatch		#specify the service
      - AWS_ACCESS_KEY_ID="test"
      - AWS_SECRET_ACCESS_KEY="test"
      - AWS_DEFAULT_REGION=us-east-1
    
    volumes:
      - "${LOCALSTACK_VOLUME_DIR:-./localstack_data}:/var/lib/localstack"
      - "/var/run/docker.sock:/var/run/docker.sock"
    

save and quit

```

### Run the localstack container using "docker-compose":

```
### Run the Localstack Container: 

docker-compose up -d 

```


```
docker ps

CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS                  PORTS                                        NAMES
1226fd4baa8b   localstack/localstack:latest   "docker-entrypoint.sh"   7 minutes ago   Up 7 minutes (healthy)   0.0.0.0:4510-4559->4510-4559/tcp, :::4510-4559->4510-4559/tcp, 0.0.0.0:4566->4566/tcp, :::4566->4566/tcp, 5678/tcp   localstack

```


```
### To enter a Docker container: 

docker exec -it localstack bash
apt update -y
```



### Run Localstack container using "docker run" (Optional):

```
docker pull localstack/localstack
docker pull localstack/localstack:2.3.2
```

```
docker images

REPOSITORY              TAG       IMAGE ID       CREATED        SIZE
localstack/localstack   latest    f29c662ab21b   7 hours ago    1.09GB
localstack/localstack   2.3.2     f7c3a25c88a7   7 weeks ago    1.2GB
```


```
### Run a Docker Container:

docker run --name localstack -dit -p 4566:4566 -p 4510-4559:4510-4559 localstack/localstack

```


```
docker ps

CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS                    PORTS                                 NAMES
93374dcc63cb   localstack/localstack   "docker-entrypoint.sh"   59 seconds ago   Up 56 seconds (healthy)   0.0.0.0:4510-4559->4510-4559/tcp, 0.0.0.0:4566->4566/tcp, 5678/tcp   localstack

```

```
docker logs -f localstack
```

</details>


---
---


# Part-2:

After the installation is complete, then lets proceed and configure the AWS CLI using the commands below.

```
### AWS CLI Configure:

aws configure 

AWS Access Key ID [None]: test
AWS Secret Access Key [None]: test
Default region name [None]: us-east-1
Default output format [None]:
```

```
### Check credential:

cat ~/.aws/credentials
```

```
### Check Region: 

cat ~/.aws/config
```

```
### Check configuration: 

aws configure list

```


## Some of AWS services Example:

### Creating S3 bucket:

```
### Lets create an S3 bucket of "testbucket" and interact with the API:

aws --endpoint-url=http://localhost:4566 s3api create-bucket --bucket testbucket

output:
------

{
    "Location": "/testbucket"
}

```


```
### See the list of buckets:

aws --endpoint-url=http://localhost:4566 s3api list-buckets

output:
------

{
    "Buckets": [
        {
            "Name": "testbucket",
            "CreationDate": "2023-11-25T17:49:50+00:00"
        }
    ],
    "Owner": {
        "DisplayName": "webfile",
        "ID": "75aa57f09aa0c8caeab4f8c24e99d10f8e7faeebf76c078efc7c6caea54ba06a"
    }
}

```


```
### Create a "test.txt" file and copy it to the testbucket:

touch test.txt

aws --endpoint-url=http://localhost:4566 s3 cp test.txt s3://testbucket


output:
------

upload: ./test.txt to s3://testbucket/test.txt


```


```
### Check file is uploaded to the testbucket:

aws --endpoint-url=http://localhost:4566 s3api list-objects --bucket testbucket

output:
------

{
    "Contents": [
        {
            "Key": "test.txt",
            "LastModified": "2023-11-25T17:51:27+00:00",
            "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
            "Size": 0,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "webfile",
                "ID": "75aa57f09aa0c8caeab4f8c24e99d10f8e7faeebf76c078efc7c6caea54ba06a"
            }
        }
    ],
    "RequestCharged": null
}

```



### Host a static Website on S3:

```
mkdir websites
vim ./websites/index.html

Static Website fof S3 on testbucket.

save and quit
```


```
### Create a bucket policy and allow public access to all of its contents. So create a file named bucket_policy.json in the root directory:

vim bucket_policy.json

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::testbucket/*"
        }
    ]
}

save and quit
```


```
### Sync the websites directory that contains our files to the S3 testbucket:

aws --endpoint-url=http://localhost:4566 s3 sync ./websites/ s3://testbucket

output:
------

upload: websites/index.html to s3://testbucket/index.html

```


```
### Finally, we enable static website hosting on the bucket and configure the index and error documents:

aws --endpoint-url=http://localhost:4566 s3 website s3://testbucket/ --index-document index.html --error-document error.html
```



### Web Console:

Website is now hosted on the emulated S3 bucket. So lets browse as like AWS: "http://<BUCKET_NAME>.s3-website.<REGION>.amazonaws.com", and in LocalStack, the S3 website endpoint follows the following format: "http://<BUCKET_NAME>.s3-website.localhost.localstack.cloud:4566".

You can navigate to looks like: "http://testbucket.s3-website.localhost.localstack.cloud:4566"


```
### Check URL in the Linux curl command: 

curl http://testbucket.s3-website.localhost.localstack.cloud:4566


output:
------

Static Website for S3 on testbucket.

```



---
---




# Part-3:

### Install AWSLOCAL Python Module:

"awslocal" is a wrapper around the AWS CLI to be used with LocalStack to shorten the command (in other words, less typing). It also supports the command completion feature of AWS CLI.

The "awslocal" CLI allows you to easily interact with your local services without having to specify "--endpoint-url=http://..." for every single command.



<details>
  <summary>For AWSLOCAL:</summary>


```
### To confirm AWS CLI installation:

aws --version
```


```
### To check Localstack:

localstack --version
```


```
### Install awslocal modules:

pip3.8 install awscli-local

```


```
### To verify:

awslocal -h

```

### Creating S3 bucket:


```
### Create an S3 bucket of "testbucket2" and interact with the API:

awslocal s3api create-bucket --bucket testbucket2


output:
-------

{
    "Location": "/testbucket2"
}

```


```
### See the list of buckets:

awslocal s3api list-buckets
awslocal s3api list-buckets --region us-east-1

output:
-------

{
    "Buckets": [
        {
            "Name": "testbucket",
            "CreationDate": "2023-11-25T17:49:50+00:00"
        },
        {
            "Name": "testbucket2",
            "CreationDate": "2023-11-25T18:32:46+00:00"
        }
    ],
    "Owner": {
        "DisplayName": "webfile",
        "ID": "75aa57f09aa0c8caeab4f8c24e99d10f8e7faeebf76c078efc7c6caea54ba06a"
    }
}


```


```
### Create a "test2.txt" file and copy it to the testbucket2:

touch test2.txt

awslocal s3 cp test2.txt s3://testbucket2


output:
------

upload: ./test2.txt to s3://testbucket2/test2.txt

```


```
### Check file is uploaded to the testbucket2:

awslocal s3api list-objects --bucket testbucket2

output:
------

{
    "Contents": [
        {
            "Key": "test2.txt",
            "LastModified": "2023-11-25T18:41:51+00:00",
            "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
            "Size": 0,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "webfile",
                "ID": "75aa57f09aa0c8caeab4f8c24e99d10f8e7faeebf76c078efc7c6caea54ba06a"
            }
        }
    ],
    "RequestCharged": null
}

```


### Host a static Website on S3:
Now, Sync the existing websites directory to testbucket2.


```
### Create a bucket policy and allow public access to all of its contents. So create a file named bucket_policy.json in the root directory:

vim bucket_policy.json

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::testbucket2/*"
        }
    ]
}

save and quit
```


``` 
### Lets, now attach the policy to the bucket:

awslocal s3api put-bucket-policy --bucket testbucket2 --policy file://bucket_policy.json

```


```
### Sync the websites directory that contains our files to the S3 testbucket2:

awslocal s3 sync ./websites/ s3://testbucket2


output:
------

upload: websites/index.html to s3://testbucket2/index.html

```


```
### Finally, we enable static website:

awslocal s3 website s3://testbucket2/ --index-document index.html --error-document error.html
```


### Web Console:
Browse to "http://testbucket2.s3-website.localhost.localstack.cloud:4566"


```
### Check URL in the Linux curl command: 

curl http://testbucket2.s3-website.localhost.localstack.cloud:4566


output:
------

Static Website for S3 on testbucket.

```


</details>



### Important Links:
- [LocalStack Documentation](https://docs.localstack.cloud/getting-started/installation)
- [DockerHub LocalStack](https://hub.docker.com/r/localstack/localstack)
- [Configuration of LocalStack](https://docs.localstack.cloud/references/configuration/)
- [Coverage of LocalStack](https://docs.localstack.cloud/references/coverage/)
- [LocalStack Workshop](https://www.youtube.com/watch?v=SYCeM-Q6nRs)
- [Static-website](https://docs.localstack.cloud/tutorials/s3-static-website-terraform/)


Remember, LocalStack is a great tool for local development and testing, but it might not perfectly replicate all AWS behaviors. Testing thoroughly in both environments is crucial before deploying to production AWS services. LocalStack has gained popularity among developers due to its ability to speed up development cycles, improve testing reliability, and reduce the complexities associated with cloud development.

