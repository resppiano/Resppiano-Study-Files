# AWS services

## Global

Regions are physical locations like Northern VA

availability zones are datacenters, there are at least 2 per regaion

edge locations are caches for CDNs, there are over 100 of these

## Compute

EC2 is elastic vms

ECS is containers

Elastic Beanstalk is "for developers who don't understand AWS" sounds like it's kinda like heroku

Lambda is functions as a service (serverless)

Lightsail is Virtual private servers. Gives you a server with a fixed IP address and shell access. Watered down EC2

Batch is for batch computing (not an exam topic)

## Storage

S3 "simple storage service" object based storage. Put stuff in buckets.

EFS elastic file system. Network attached storage that you can mount to vms

Glacier is data archival. Stuff you may need to only check every year or so, but it's cheap

Snowball is for moving large amounts of data into the datacenter. Ship physical disks to the data center. Big ol' box

Storage gateway is virtual applicances. VMs that are in your head office that get replicated into s3

## Databases

RDS is relational database service. Mysql, aurora, postgres etc.

DynamoDB is non-relational data.

Elasticache caches commonly queried things from your database server. Cache in from of you DB

Redshift is for datawarehousing. Complex queries.

## Migration

AWS migration hub lets you track services while you migrate them into aws

Application Discovery Service detects what applications you have and qwhat their dependencies are (sharepoint -> mysql etc.)

Database migration Service lets you migrate from on prem databases into AWS

Server migration service helps you migrate from physical serviers into AWS

Snowball is here too. Part of both Storage and migration

## Networking and content delivery

VPC is virtual private cloud. Like a virtual datacenter. Networking stuff is here. Very fundamental part

Cloudfront is aws's CDN

Route53 is DNS

API Gateway creates web api that routes to other services

Direct Connect (important for exam) runs a dedicated line from corporate office or datacenter directly into VPC

## Developer Tools

(Note these don't come up in exam)

CodeStar gets a group of developers working together easily. CD toolchain

CodeCommit VCS git repository storage

CodeBuild is CI

CodeDeploy is application deployment to EC2, on prem, lambda etc.

CodePipeline is Continuous Delivery pipeline

Xray is request tracing for deployed code

Cloud9 is IDE in the aws console (aquired by AWS released at re:invent 2017)

## Management Tools

Cloudwatch monitoring tool. Big in sysops/adminitrator area

Cloudformation is important for solutions architects. A way of scripting infrastructure. Templates for sites/infrastructure. !!! IMPORTANT !!!

CloudTrail is auditing/change logging for aws environment. !!! IMPORTANT !!!

Config monitors configuration of entire aws environment with point in time snapshots. Has visualizers

Opsworks like elastic beanstalk but more robust. Automates with Chef and Puppet

Service Catalog manages a catalog of IT services that are approved for use in your large company

Systems manager is an interface for managing resources. Example use is rolling out a security update to thousands of ec2 instances. Not in exams

Trusted Advisor is important for security/solutions architects. Know the difference between this an inspector. Gives advice accross disciplines. Tells if you've left ports open, not using services in the right way. Sounds kinda like Lint for aws

Managed Services managines things for you like autoscaling (not entirely clear) doesn't seem super important for the moment

## Media services

This is a brand new category as of 2017 (None of these are going to be in exams)

Elastic Transcoder connverts/resizes videos (this one is not new)

MediaConvert VoD for broadcasting

MediaLive hq video streams

MediaPackage prepares video for delivery over the internet

MediaStore media optimized storage

MediaTailor targeted advertising in video streams

## Machine Learning (Not part of exams)

SageMaker Deep Learning for developers

Comprehend Sentiment analysis in data

DeepLens artifically aware cameras (on device, not on cloud)

Lex powers Alexa. Communication service (chat AI)

Machine Learning regular ol machine learning platform. Distinct from deep learning, which is neural networks.

Polly takes text and turns it into speech. Apparantly the voices are actually real good.

Rekognition will tell you what's in a file; images, video etc.

AmazonTranslate just like Google Translate

Amazon Transcribe automatic speech recognition

## Analytics

Athena lets you run SQL queries on the stuff in your s3 buckets. Works on excel spreadsheets etc.

EMR elastic map reduce (in exam) processes large amounts of data with mapreduce

CloudSearch amazon search service

ElasticSearch service hosted elasticsearch instances

Kinesis (important for exam) ingestion service for large amounts of data

Kinesis Video Streams exactly what it says on the tin

Quicksight is Business Intelligence

Data Pipeline moves data between AWS services (in exam)

Glue is an ETL service (too new for exams)

## Security Identity and Compliance

IAM Identity Access Management. Starting point for evertying. Manages accounts/permissions

Cognito does device authentication

GuardDuty monitors for malicious activity on AWS account (not in exams)

Inspector agent that lives on VMs and lets you run tests to detect vulnerabilities etc. generates a report of issues.

Macie scans s3 buckets for PII

Certificate Manager free SSL certificiates (if you're using route 53)

CloudHSM Hardware Security Modules for storing keys

Directory Service integrates microsoft AD with AWS

WAF web application firewall prevents things like xss and sql injections. Layer 7 security.

Shield DDOS mitigation (there is also an advanced version of this for if you're like reddit or something)

Artifact Auditing and compliance. On demand portal for downloading those reports.

## Mobile services

Mobile hub is a management console for your mobile app. Lets you use the aws sdk in your apps (not in exam)

Pinpoint targeted push notifications (not in exam)

AWS AppSync updates data in web and mobile apps in real time (sounds like firebase) (not in exam)

Device Farm app testing accross many devices (not in exams)

Mobile analytics is just what it says on the tin (also not in exams)

## AR/VR

"Sumerian" is the code name for this effort. Idea is that this is a "common language" and toools for these environments

Obvs this is not in any of the exams

## Application Inegration

Step functions is management of Lambda deployments

Amazon MQ is basically RabbitMQ

SNS notifications

SQS Queuing. This was the first ever AWS service.

SWF Workflow service. Can involve humans and computers both

## Customer engagement

Connect is contact center as a service "Call center in the cloud" Not on exam

SES is Simple Email Service

## Business Productivity

Alexa for business dials into meetings, open it tickets, etc. Seems gimmicky IMO. Not in exams

Chime is video conferencing

Work Docs is dropbox for aws (this comes up in exams a little)

Workmail is like Office 365

## Desktop and App streaming

Workspaces is a VDI (run OS in amazon cloud and stream to device)

AppStream 2.0 streams an individual app to device. Sounds liek Citrix

## Internet of Things

These are real new, not gonna be on exams

iOT

iOT Device Management fleet management for iOT

Amazon FreeRTOS OS for microcontrollers

Greengrass

## Game development

GameLift is a game development service. Not in exames