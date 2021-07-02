# SAP-C01 Study Notes

🛑 Please note **my** notes are most likely useless for **you**.

## S3
For Glacier, through aws console, you can only create and delete vaults, all other operations require CLI or code.

## EC2
EC2 placment groups:

* Cluster – packs instances close together inside an Availability Zone. This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typical of HPC applications.
* Partition – spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.
* Spread – strictly places a small group of instances across distinct underlying hardware to reduce correlated failures.

Both ALB and NLB support SNI.

Increasing EBS by 1T add 3000 iops.

One can use session manager(on system manager, but from ec2 console) to rdp/console to ec2 without opening ec2 port (rdp or ssh).

## SQS

sqs visibility timeotu, delay queue, message timer.

Message timer can be used to delay individiual message, 15min max.

Deley Queue is for whole queue, also 15min. max.

## Auto Scaling

These services have auto scaling: EC2, ECS, DynamoDB, Aurora (replicas).

## Elastic Beanstalk

You can rebuild terminated environments with 42 days.

EB environment tiers: web server vs worker

## SnowXyz

* Snowmobile >10PB <100PB
* snowball <10PB
* snowball edge: 80TB, does encryption.

## VPC

VPC peering will route to more specific route first.

One VPC can peer max 125 others (default 50).

VPC peering can be cross-region and cross-account.

Understand VPC Inbound Resolvers

egress-only internet gateway is for IPv6 only.  Ipv6 is global by default. to prevent internet from accessing ipv6 resource, use egress-only gateway.

one can expand VPC ip range by adding upto 5(or 4?) secondary cidr blocks.

Nacl is at subnet level.

security group default is deny all, you specify allowed access. can not deny ip.

Nacl can specify allow and deny, can deny on ip.

when to use transit VPC:

* private networking between 2 or more region
* cross-account aws usage
* share connection: VPCs, on-premise, parters.

Bastion host allow ssh/rdp access from internet, it act as a jump server.

promiscuous mode is for packet sniffing.  AWS networking doesn't do promiscous mode.

## CloudFront

CloudFront can be used to front on-premise site (custom origin).

cloudfront origin must be domain/path, it can not be IP address.

CF can geo-restrict.

Cloudfront can not do user-agent based routing, need lambda@edge for this.

## DirectConnect

DirectConnect can connect to private subnet in VPC via virtual private gateway/virtual interface.

## Redshift

Redshift has not read replica.  It has auto backup to S3.

It does auto-snapshot, but snapshot is not cross-region by default.

Redshift **shift** away from Oracle(red).  It's mostly for data warehousing.

Redshift Spectrum and query on S3 directly.

## Lambda

concurrent execution limit is 1000 and can be increased with request.

If a job takes more than 15min, it can not be run on Lambda.

## SQS

FIFO can do 300 t/s, batch mode can do 3000 t/s.

FIFO high throughput can do 3000, batch mode 30000.

If there's no order requirement, should not consider FIFO, (couldn't be distractor)

## DynamoDB

The max item size in DynamoDB is 400KB.

DAX is in-memory cache for DynamoDB.

## RDS

## Kinesis

Kinesis data streams store data for up to 7 days. so we can run 2nd application up to 7 day after teh first appliation

Kinesis data blob size is 1MB, each shard can do 1000 puts/s, so throughput is not usually a problem.

Kinesis data stream retention min/default:24 hours max: 365days (used to be 7 days)

KDS has enhanced fan-out, it supports HTTP/2.

## API Gateway

29 s timeout

10MB max payload

## Migration

DMS supports DB2, LM also support DB2.

DMS can be used to migrate from s3/csv to DynamoDB.

SCT can copy data too (using SCT replication agent and SCT data extraction agent), not just schema conversion.

Storage gateway: cached volume 1024T max, stored volume: 512T max

## More to reads

* faq:AWS Backup
* faq:AWS CloudEndure
* blog: Implementing Canary Deployments of AWS Lambda Functions with Alias Traffic Shifting

## Others

Global Accelerator can be used for blue/green in place of route 53 if DNS caching is a concern.

With global Accelerator, you are give 2 static IP to connect to your (multi-region) aws service/resource.

SAM (with CodeDeploy) can do traffic shifting for Lambda.  SAM is actually an extension of CloudFormation.

WAF can be on cloudfront, ALB, API Gateway, AppSync.

PrivateLink vs Transit VPC.

Don't confuse fault tolerent with disaster recovery

Alexa for Business is for organizations and its employee, not for interacting with customers.

RDS has events.

NAT gateway doesn't support IPv6.

Oracle RAC is not supported by RDS.

If DB is big, short interval snapshot may not be feasible for short RPO. For example, you can not do 30TB DB snapshot in 5 min.

Amazon Connect -> call center.

RDS Read Replica is async.

ElastiCache can be used for DynamoDB, but DAX is prefered, DAX can do both read and write.

CodePipeline source providers are: CodeCommit, ECR, S3, Bitbucket, Github

AWS taskcat is a tool that tests AWS CloudFormation templates

IAM certificate store (not recommended to use), can be accessed from CLI, 

AWS CDK let you do infrastructure as code in TypeScript, Python, JS, Java, C#, the stack is translated to a CF stack.

A Spot fleet can have differenct instance types, and have higher chance to acquire instances than just spot instances.

Note the difference between DDOS mitigation and detection/prevention.

AWS Application Migration Service (MGN) replaces CloudEndure and SMS.  This is new (as of 05/2021) and will not show up in exam.

CloudHub let you connect different on-premise sites through AWS VPN, as well as site-

DataSync: data/storage transfer, aws-aws, aws-onprm

WorkDocs: like DropBox or box.com

Data Pipeline: ETL, copy RDS to S3, copy from S3 to RedShift etc. can be used for on-prm -> aws. can run on Hive, Pig.

Glue: also ETL, run on Spark, job use Scala or Python

DocumentDB: compatible with MongoDB, store/manage/index JSON data.

Inspector: automate security vulnerability assessments, for EC2 it need agent installed.

X-Ray: need agent except EB.

There is CloudSearch

Shield Standard

FSx for Windows File Server is like samba drive.

FSx for Lustre is for high performance computing, GigaByte/s, million iops.

EFA - for HPC 

S3 Access points Amazon S3 Access points simplify managing data access at scale for shared datasets in S3. Access points are named network endpoints that are attached to buckets that you can use to perform S3 object operations

Amazon Macie is a security service that uses machine learning to automatically discover, classify, and protect sensitive data in AWS

Amazon Inspector automatically assesses applications for exposure, vulnerabilities, and deviations from best practices. 

Macie: data, Inspector: application

Amazon Neptune is a fast, reliable, fully managed graph database service

Route53 supports DNSSEC.

Resource Access Manager RAM share VPC/subnet amoung multiple accounts, resource still ones own.

AppSync does GraphQL api over websocket.

Note if ids/ips, make sure to satisfy both, agent running in instance can only detect, not prevent.

cloudwatch events is now EventBridge.

SSO support saml2 and web only(no desktop/mobile app)

Cloudformation template can be used from one region to another.

Step functions coordinate multiple aws services into serverless workflow (lambda).

Multi-AZ RDS standby can not be used for r/w.  It's just a backup.

Read replica is not automatically promoted to primary if primary failed.
