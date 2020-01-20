# EC2 notes

resizable computing in the cloud

"Elastic Compute Cloud"

get new servers in minutes

capacity can scale up and down, and also scale out w/ a load balancer

pay for the capacity that you actually use

## pricing options

On demand - fixed rate by the hour (or second) with no commitment

Good for no commitment, unpredicatble workload, and for dev/testing. We'll use this for learning

Reserved - you get a capacity reservation w/ amazion. get a discount on the hourly charge but you're locked in for your term

Good for predictable, steady usage. Can pay up front for discounts. up to 75% cheaper than on-demand. Can also have instances that are convertable between different types, or scheduled reservations.

Spot - lets you bid a price for your instance capacity. Price moves around all the time like a stock market, but it can save you a lot if your timing is flexible

Good for if your app is only profitable at low compute prices. i.e. genome computing at 4 AM on Sundays

Dedicated hosts - physical servers dedicated for your own use. Lets you use server-bound licenses.

Good for if you aren't allowed to use multi-tenant virtualization because of laws.

## Instance types

There's a lot of instance types (around 11) don't need to memorize them for this exam but some of the others might need them

T2 is for low cost, general purpose computing. That's what we're going to use for this class.

Letter is for type, number is for "versions"

pneumonic is FIGHT DR MCPX. Don't need this yet but it is interesting.

## EBS

Virtual disk in the cloud.

Block storage that you can attach to ec2 instances.

they are placed in specific AZs and it automatically replicated.

You get a root device volume attached to your EC2 instance, and then you can mount more volumes.

Types:

* GP2 - general purpose SSD. goes up to 10k IOPS plus burst if necessaey
* IO1 - Provisions SSD. Used for if you have more than 10k IOPS. Used for databases and stuff
* ST1 - throughput optimized HDD. Used for big data. Can't be used as a boot volume.
* SC1 - Cold HDD. Low cost. Can't be used as boot
* Magnetic - low cost boot drive. Deprecated but still available.

## Exam tips

Know 4 pricing models: On demand, reserved, spot, and dedicated hosts

Spot will not charge you for the fuill hour if AWS terminates you, but it will charge you for the full hour if YOU terminate it

FIGHT DR MCPX

EBS types: General SSD, provisions IOPS SSD, Magnetic throughput optimized, magnetic cold hdd, and previous generation magnetic.

------

## First lab: launch an ec2 instance

start by clicking launch instance. You get a list of default AMIs

picked the Amazon Linux AMI, and a t2.micro

you can select a VPC in here, this will be important later. Using the default VPC for now.

subnets map to availability zones 1:1

shutdown behavior can set whether the VM gets destroyed or just stopped at an OS shutdown.

Can specifiy tenancy and monitoring.

Can add bootstrap scripts (will get into this later)

next section adds EBS volumes

can also add tags. Will get into this later (helps control costs). Ideally you want to tag everything

next is security groups _security groups are just virtual firewalls_

Can make a new security group here. Added ssh, http, and https. Restricted ssh to my IP address (actually didn't do this because work network). Left other sources open from everywhere.

Need to create a key pair (or select an existing one) as the final step in creating. Created one and downloaded it.

Launched the instance, it starts off in the pending state. We can check out what we set in the console.

sshed into the ec2 instance (had to chmod 400 the key for that)

`ssh ec2-user@<public-ip> -i pair`

had to use mobile to shell into the instance... so that's not ideal

`sudo su` to get root

`yum update` to apply updates

`yum install http` to get apache

`service httpd start`

put a file in `/var/www/html` and you can see it online!

Part 2 of the lecture starts here

looking in console at the various fields in description tab on tha console

because termination protection was turned on when we set this up, it won't let us terminate. You have to go in and manually turn off the termination protection

Looking at the status checks tab you can see the system status check and the instance status check

System status check checks if you can get network packets to the instance. Just checking the underlying hypervisor/networking.

Instance status check sees if you can actually get traffic to the OS. Instance status check failures can *probably* be fixed with a reboot. This is more of a sysops administrator question.

Monitoring tab has default, every 5 minutes checking. Lots of metrics from cloudwatch. Can turn on detail monitoring for an additional charge. Later on in the cloudwatch section we'll go into what all of these are.

Tags tab shows us what we set up earlier.

Terminating the instance in order to delete it

Looking at the reserved instances section just to see the menu. It'll price you right there and let you buy it.

looking at volume encryption, you can encrypt additional volumes, but not the root volume at launch. To encrypt the root volume you need to create a backup copy of the root volume and encrypt it while making the copy. This will be covered in a later lab

### Lab summary

Can create an ec2 instance

Can create ssh keys

Can ssh into the new instance

Learned about security groups

applied updates and installed apache on ec2 instance

Looks at ec2 details in console

### Know for exam

Termination protection is turned off by default

Default action when an instance is terminated is to delete the attached ebs instance. Can change this setting.

EBS root volumes cannot be encrypted, but you can use 3rd party tools, or you can create a copy of the AMI and encrypt it there.

Additional volumes *can* be encrypted.

## How to use putty for windows lecture

SKIPPED!

## Security groups lab

A security group is a virtual firewall

Every EC2 instance has at least one security group.

created a new ec2 instance with the security group from last time, installed apache and added a basic html file

checked security group rules in the console.

Delete http rule, and *immediately* we are not able to access the web page.

Know for exam: security rule changes apply immediately.

Added rule back and we were able to get back in.

Deleted outbound rule, and we were still abel to load the web page.

Security groups are stateful. All inbound rules will automatically be allowed out.

Later, with network access control lists, those are stateless so you need inbound and outbound.

Security group rules are stateful (not sure of the connection between state and inbound-outbound mapping but we'll probably come back to it later)

In security groups, you can't *deny* specific traffic, you can only allow it. It's a whitelist.

There's a default security group in each AZ, and it's set up by default to allow all traffic from instances in the same security group.

added RDP and MYSQL/Aurora to default group

in ec2 console, Actions -> networking -> change security groups to add another group

we added that default security group to our ec2. You can add multiple security groups to an instance.

VPC: stateless, Security group: stateful. It's been repeated a lot.

## Upgrading EBS volumes

created an ec2 instance with 3 different types of EBS volumes attached to it

it wasn't free so I cancelled creating it

EBS volumes have to be in the same AZ as the EC2 instance. Makes sense because latency.

You can tell which volumes are root devices because they have a "snapshot"

You can change the volume type and increase the size through the volumes console. (except on standard magnetic, can't change size on that)

to move a volume to another AZ (common exam question) you can create a snapshot in the volumes console (takes like 5 minutes)

Then from the snapshot you can create an image, volume, etc.

Create a new volume and you can change the type, size, and AZ. This is how you could move a volume to a new zone and thus a new instance.

Can also copy the snapshot to a new region. You'd have to do this to move an ec2 instance from one region to another.

Creating an image from a snapshot lets you boot an EC2 image from that instance. Snapshots are from backups, images are used for creating new ec2s

Image == AMI

Can create a snapshot then an image from a snapshot, or you can create an image directly from an ec2 instance if you'd prefer

By default, the root volume gets terminated when your EC2 is terminated, BUT the other attached volumes won't be terminated by default.

### Exam tips

Volume = Virtual hard disk

Root device = boot disk

Snapshots are saved in S3. They are a point in time copy of a Volume.

Snapshots are incremental, only the blocks that have changed since your last snapshot are moved to S3.

You shoudl stop an instance before taking a snapshot *probably*

you can create an AMI from Instances and Snapshots

You can change EBS volume sizes on the fly (this is relatively new)

Volumes will ALWAYS be in the same availability zone as the EC2 instance.

To move an EC2 to another AZ/Region, take a snapshot or an image, and then copy that to another region.

Security wise, snapshots of encrypted volumes are encrypted automatically. Same goes when restoring a volume from an ecrypted snapshot

You can share snapshots, but you can only do it if they are unencrypted

copying images/snapshots is SUPER important for the exam so know how to do it.

## RAID, Volums & Snapshots

instructions to create a RAID group with aws volumes, and launch windows vms.

RAID types:

* RAID 0 - Striped, No Redundancy, Good Performance
* RAID 1 - Mirroed, Redundancy
* Raid 5 - Good for reads, bad for writes, AWS discourages making RAID 5's on EBS
* RAID 10 - striping and mirroring, good redundancy and good performance

You'd use a RAID (probably 0 or 10) if you need better Disk I/O

### Lab

added RDP ports to our security group

created a free windows ec2 instance

(it actually cost a little money becuase we added 4 additional volumes)

RDP into the instance with the user `administrator` and the password is from the platform

have to use your key file to decrypt the passwor din the aws console. But it's making me wait 5 minutes first

used remote desktop to get into the machine we launched

go into disk management to see your mounted volumes. Mine were not online or initialized yet so I put them up

created a new striped valume with all the disks. Now we have a RAID array with better IO than the root volume

### How to take a snapshot of the RAID array?

problem is that a snapshot excluded data held in caches

This is usally fine, but in RAIDs it can cause problems.

Have to freeze the file system, unmount the RAID array, or shut down the ec2 instance. These ooptions will stop the app from writing to disk and it will flush all caches.

This is important if there's a question about snapshotting a RAID instance. Easiest way to go is shutting down the EC2 instance before the snapshot.

## Create an AMI - Lab

Start by stopping the ec2 instance

Then go to volumes, and create a snapshot

Once it's created, we copy it to another region, and encrypt it while copying it

Change resgions and you can see your new snapshot

Under actions, make an image off of the snapshot

from the ami screen, go to launch

In the storage tab you can see that the root device volume is encrypted

launch instance and it will be copied from region to region.

There are AMI availabel to buy/rent per hour with software built in. There are also community AMIs. However OTS ami's are not encypted. Encrypted AMIS will always be private

Checked that the new instance was up, and then terminated it, and cleared out stuff in London region

### notes

Snapshots of encrypted volumes are encrypted automatically, same goes for volumes that are restored from encrypted snapshots

You can't share encrypted snapshots

unencrypted snapshots can be shared, or even sold

AMIs are base images

They are an important AWS building block, so make sure you understand them for the exam

## AMI types

2 different types: EBS backed, and instance store backed.

Can select an AMI based on Region, OS, Processor architectore, or storage for rood device.

Instance store is Ephermal, EBS backed is not.

On default amis, root devices are always ebs

under community AMIs, you can look for one that has an instance store. This limits what instance types you can use (can't use micro)

At launch time, you can attach sdditional instance store volumes, but once it's created you can't add more. You can only add EBS

With the EBS backed instance, you can stop it, take a snapshot, etc. You can't stop an instance store based volume. It's less durable. If the underlying host fails you will lose your data.

You can *reboot* an instance store and not lose the data though.

Instance stores are older, EBS is newer.

You can detatch an EBS volume and attach it to another instance if you want.

Honestly not sure why you'd use instance store. (looked it up, you'd use it for freqwuently changing data like buffers or caches. Advantage it that it's physically attached to teh host that the EC2 is on)

## Load balancers (lecture)

A load balancer is a virtual appliance that balances the load of web applications over muliple resources (web servers) So that no 1 resource gets overwhelmed

### Application Load Balancers

Load balances http and https traffic. They are application aware and operate at Layer 7. It can make intelligent routing decisions like determining the traffic source etc.

Network Load Balancers are for TCP traffic where extreme performance is required. It operates at Layer 4 (Connection Level) and can do millions of requests per second.

Classic Load Balancers (also called Elastic Load Balancers) It's basically the depracated version of ALBs. Can do layer 7 and layer 4 stuff. But the layer 7 stuff is not as smart as ALBs though

Load balancers will respond with a 504 error if your application stops responding.

X-Forwarded-For Header is the http header that contains the IP of the original requestor, that's how your application will see the original IP when it's going through a load balancer.

## Load balancer lab

turned on that web server ec2 instance again to get a basic web page up. also add a healthcheck.html with just "OK" in it

click on load balancers in the console to create one

making a classic load balancer first

don't make it internal, because it's balancing externally

set it up to accept on port 80 and pass to port 80

we get yelled at about not using ssl, but we can ignore that for this example.

set the port and path for health check to 80 and healthcheck.html

set the timeouts and thresholds to be low so that we can see the changes fast

add our ec2 instance to the ELB, and add tags

it's easy to forget ELBs, so don't forget to delete them

created ELB, waiting a couple minutes for it to get picked up and for the health checks to settle

deleted the healthcheck file to put it out of service

put it back and see in the console that it goes back in service

copy pasted the dns name from elb console into browser and saw that traffic was forwarded

ELB does *not* get an IP address (in the console). Amazon manages you IP for ELBs because the public IPs might change, so they want you to use the DNS name.

Creating an application load balancer

you pick availbility zone for the ALB

ALB setup is similar to ELB, but there are more options

You have to set up a separate target group from the ALB. It's decomposed down into 2 pieces

takes a minute for it to provision the ALB

There's a deeper course on ALBs cuz there's a lot going on here

For exam, read teh ELB FAQ because it's important.

Deleted ELB and ALB

## Cloud Watch

we techinically already used this when we set up that $10 billing alarm

start ec2 instance and go to monitoring tab

Basic monitoring is default, and it checks metrics every 5 minutes.

Detailed monitoring goes every 1 minute, but it costs more

next go to the cloudwatch section

create a dashboard

can add a bunch of widgets to the dashboard

added a text widget, this just displays some markdown

added a line widget, which will let you plot *any* of the available metrics in your region

need to know for exam what metrics are available by default.

They are CPU related, disk related, network related, and Status checks

CPU: utilization, credit balance, credit usage, and surplus credit balance/charges

Disk: read/write Ops/bytes

Network: In/out + packets in/out

Status check: failures/ instance failures.

RAM is *NOT* tracked by default. Have to set up that metric as custom (needs code)

added some stack graphs and numbers too.

Now creating an alarm

Made it for CPU utilization. Set it up to notify if it goes over 80% it notifies

set up a new notificaiton list (will have to confirm email)

Events: they help you respond to state changes, and lets you route those events when one happens. Example: Lambda functino to update DNS when an EC2 instance enters running.

An event gets pushed every time there is a state change.

Events are out of scope for the exam.

Logs: lets you monitor ec2 instances at the app level. Apache logs, kernal logs, etc.

You install an agent on your ec2 instance, and it passes monitor data back to cloudwatch logs, then you can view them inside the portal.

Logs are also not in the exam, but good to know.

Metrics section lets you view your metrics in one place, as opposed to a dashboard.

### Exam tips

standard monitoring 5 mins, detailed 1 min

Dashboards lets you make... dashboards

Alarms are notificaiton when a metric goes over a threshold

Events are reactions to state changes

Logs are aggregating monitoring and storing logs in cloudwatch

Cloudwatch is *not* cloudtrail. Cloudwatch is logging and monitoring, Cloudtrail is for auditing.

## AWS cli

Launching a new instance to start from scratch

Using amazon linux since it has the cli installed

creating a new CLI user

ssh into new instance and `aws configure`

put in the key and secret for that new user

`aws s3 ls` works now!

look in the `~/.aws` directory to see your keys

This is actually pretty bad, bcause someone with your key could open up the file and see your credentials, which have access to *EVERYTHING*. Next lab will be aboout roles

going to self destruct the instance

`aws ec2 describe-instances`

coped instance id for the running instance

`aws ec2 terminate-instances` with the id to kill the instance

go back and delete that iam user

## IAM roles and ec2

IAM roles is how you can get aws cli access on an ec2 instance without risking exposure of your keys

going to create anec2 instance and give it a role so that it cazzn access s3

in IAM console, creating an s3 admin access role

remember that roles are created globally, it's not per region

launched a new ec2 instance, and gave it the s3 admin access IAM role

from here you can go in and click on the policy to get a full view of it

also, you can go into the console and actually *attach* a new role to a running intance. This is pretty new.

sshed into the new instance

`aws s3 ls` works! This is because we attached the role

Note that there are *not* any credentials stored in `~/.aws`.

even if you aws configure and set, say, a default region, it won't set the creds

## AWS cli and regions

this is a whole 9 minute video saying that aws command line will default to a region that may not be what you want, and if it's wrong you shoudl pass in the `--region` argument

## Bootstrap scripts

Bash scripts that will be run when your ec2 instance starts up

made a basic html page in the text editor

made an s3 bucket and uploaded the index file to it. *did not make it public*

went to IAM and confirmed the s3 admin access role

made a new instance with the s3 access role

added a `yum update` bash script to the instance

sshed into the instance. I guess it ran those updates because I don't see it needing any.

going to set up a web server, and copy all the commands in that script here so that we can make it a bootstrap script later:

```
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
aws s3 cp s3://tyler-website-bucket/index.html /var/www/html
```

put a new instance up with this in the bootstrap script section, asnd it launched with the webserver all ready to go!

## Instance metadata

how to get stuff like IP from the command line instead of only through the console.

made a new instance with the s3 admin IAM role and ssh'd in

to get metadata you have to curl `http://169.254.169.254/latest/meta-data`

MEMORIZE THAT URL. 169 254.

`http://169.254.169.254/latest/meta-data`

you get a list of variables that you can check

you do this with startup bash scripts to do cool things. Like send your ip to a lambda that can update route53.

## Autoscaling 101

using launch configurations and auto scaling groups

made a health check html file in local text editor

threw that health check html file into the s3 bucket we made earlier for the website lab

recreating load balancer from before so we can reuse it. (it's got no targets in the target group though)

go under auto scaling -> launch configurations

give the launch configuration s3 admin access role and put in the bootstrap script from before that autopmatically sets up the website (edited script to copy entire directory from bucket not jsut index.html)

```
#!/bin/bash
yum update -y
yum install httpd -y
aws s3 cp s3://tyler-website-bucket /var/www/html/ --recursive
service httpd start
chkconfig httpd on
```

when you create the launch configuration, it does not actually make any ec2 instances yet.

Go to create an auto scaling group. Make one with 3 instances

pick your subnet (availability zone). Grab all of them if possible. It'll automatically put them in different zones

under advanced details, select your load balancer (target group) from earlier and set it to use those health checks.

jack the grace period down becuase it defaults to 300 seconds (changed to 120)

scaling policies let you automatically increase/decrease the number of instances in the group depending on rules

set up a rule (if CPU is >90 for 5 minutes, add 1 instance and give it X time to warm up)

Actually don't use the rules for this lab, juse select initial size

Can set up notifications for the group whenever instances launch, terminate etc.

after making the zone, you should see it create your 3 instances right there in the ec2 dashboard (I had to do this twice I messed up the first time)

the instances all came up and they all had the web pag:w
e working

load balancer url worked too

go in and terminate 2 of the instances to see what happens

IPs for the guys I terminated don't work anymore. ELB still works. You can see in the ELB config that 2 of the instances are unhealthy.

Waiting for like a minute, you can see it starts creating new instances!

after this, went in and deleted the autoscaling group, that will auto kill the instances that it uses

## EC2 Placement groups

This is in exams a lot so know what it is.

2 types: Clustered and Spread.

Spread is relatively new, so the exam is probably talking about clustered.

Clustered is a grouping of instances within a single AZ. Its for when you need low network latency or high throughput or both. For hadoop or cassandra or similar.

Stuff that's going to be talking to each other a lot on the network.

Only certain instances can be in a Clustered placement groups. (no micros, just big boyz)

Thing to know: Clustered is in 1 AZ. Can't be accross multiple.

SPREAD placement group is instances that are spread accross different pieces of hardware. For if your app has a small number of critical instances that need to be separated.

name for a splacement group must be unique for your account

AWS recommends you use homogenous instances in the groups. Same size + family.

Can't merge placement groups, and can't move an instance into a placement group. But you can make an AMI from an instance and then launch that into the placement group.