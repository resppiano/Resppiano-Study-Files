# [Brandon's Notes] AWS Best Practices


# Introduction
The best practices apply to both migration of existing applications and design of new applications for the cloud.


# The Cloud Computing Difference
## IT Assets Become Programmable Resources

## Global, Available and Unlimited Capacity

## Higher Level Managed Services

## Security Built In


# Design Principles
## Scalability
- Pros: economies of scale, scale in linear manner.
- Two ways to scale: *vertically* & *horizontally*.

### Scaling Vertically
- Scale by increasing the capacity and power of an individual instance. 
- Pros: easy to implement; sufficient for short term in some cases.
- Cons: will eventually hit a limit and is not always cost efficient.

### Scaling Horizontally
- Scale by increasing the number of resources.
- Better way to leverage the elasticity of cloud computing but not apply to all cases.
- Examine by scenarios: *stateless applications*, *stateless components*, *stateful components*, *distributed processing*.

#### Statless Applications
- Needs no knowledge of previous interactions and stores no session information.
- Easiest to scale horizontally: any request can be serviced by any of the available compute resource.

#### Stateless Components
- 

#### Stateful Components
- Databases are by definition stateful.
- Can be scaled horizontally by distributing load to multiple nodes with *session affinity*.

#### Distributed Processing
- When processing large amounts of data, divide a task into small fragments of work and execute them in seperate compute resources.

## Disposable Resources Instead of Fixed Servers
### Instantiating Compute Resources
#### Bootstrapping

#### Golden Images

#### Hybrid

### Infrastructure as Code

## Automation
- AWS Elastic Beanstalk
- Amazon EC2 Auto Recovery
- Auto Scaling
- Amazon CloudWatch Alarms
- Amazon CloudWatch Events
- AWS OpsWorks Lifecycle Events
- AWS Lambda Scheduled events


## Loose Coupling 
### Well-Defined Interfaces

### Service Discovery

### Asynchronous Integration

### Graceful Failure

## Services, Not Servers
### Managed Services

### Serverless Architectures

## Databases

### Relational Databases
*Amazon Relational Database Service (Amazon RDS)*

- Scalability
- High Availability
- Anti-Patterns

### NoSQL Databases
*Amazon DynamoDB*
- Scalability
- High Availability
- Anti-Patterns

### Data Warehouse
Specialized type of relational database.
*Amazon Redshift*
- Scalability

- High Availability

- Anti-Patterns

### Search
- Scalability
- High Availability

## Removing Single Points of Failure
### Introducing Redundancy
Having multiple resources for the same task can remove single points of failure. 
There are two modes: standby or active mode.
- Standby Redundancy: failover
- Active Redundancy: requests are distributed to multiple redundant compute resources. Achieves better utilization and affect a smaller population.
### Detect Failure
### Durable Data Storage
### Automated Multi-Data Center Resilience
### Failure Isolation and Traditional Horizontal Scaling
