# COMP5349 Cloud Computing — MCQ Master Revision Notes   
**Covered weeks:** Week 1, Week 2, Week 3, Week 4, Week 8, Week 11, Week 12, plus Week 13 exam structure.

---

# 0. Exam Strategy from Week 13

- **Q1: 40 marks MCQ**  
  - 20 MCQs  
  - 2 marks each  
  - single correct answer
- **Q2: 10 marks IAM identity**
- **Q3: 10 marks IAM policy**
- **Q4: 6 marks SQS**
- **Q5: 16 marks VPC/Subnet/Security Group**
- **Q6: 8 marks ASG/ALB**
- **Q7: 10 marks Assignment 2 discussion**

---

# 1. Week 1 — Intro to Cloud Computing

## 1.1 What is cloud computing?

Cloud computing means renting/sharing IT resources over the internet.

Instead of buying your own servers, storage, databases, and networking equipment, you rent them from a cloud provider.

Core idea:

> Cloud = on-demand IT resources + internet access + pay based on usage.

Important features:

- on-demand access
- shared resource pool
- scalable resources
- delivered through internet/web
- pay-as-you-use pricing
- provider manages physical infrastructure

MCQ trap:

- Renting EC2 from AWS = cloud.
- Buying/renting physical servers from Dell = not really cloud.
- Renting simple web hosting may not fully count as cloud unless it has cloud-like on-demand scalable service model.

---

## 1.2 IT resources

IT resources can include:

- physical data centres
- servers
- CPU
- memory
- storage
- networking
- operating systems
- databases
- applications
- developer platforms

Cloud turns these into services.

---

## 1.3 Service models

## SaaS — Software as a Service

You use finished software.

You do not manage:

- OS
- hardware
- runtime
- platform
- network

Examples:

- Gmail
- Google Docs
- Office 365
- Salesforce

Use when:

- you only need the application
- you do not want infrastructure control

MCQ cue:

> “User uses an application but does not control OS/hardware/network” = SaaS.

---

## PaaS — Platform as a Service

You deploy your application onto a managed platform.

You manage:

- your application
- maybe data/configuration

Provider manages:

- OS
- hardware
- runtime/platform
- network

Examples:

- Google App Engine
- managed MapReduce platform
- app framework hosting

Use when:

- you want to deploy code without managing servers

MCQ cue:

> “Consumer controls application but not OS/hardware” = PaaS.

---

## IaaS — Infrastructure as a Service

You rent basic compute/storage/network resources.

You manage:

- OS
- software
- runtime
- application
- some networking/security config

Provider manages:

- physical data centre
- physical server
- hardware

Examples:

- EC2
- EBS
- VPC

Use when:

- you need control over OS and server environment

MCQ cue:

> “Consumer controls OS, storage, deployed apps” = IaaS.

---

## 1.4 Pricing

Cloud pricing can depend on:

- time of use  
  Example: EC2 per second/hour
- storage usage  
  Example: S3 GB-month
- traffic  
  Example: data transfer out
- requests  
  Example: S3 GET/PUT/LIST
- subscription  
  Example: SaaS monthly/yearly
- quality tier  
  Example: faster retrieval, higher availability, higher durability

Important:

- Cloud cost often increases with actual usage.
- If a web shop gets 5x traffic, cost may increase roughly with usage.

MCQ trap:

> AWS costs are not only based on running time. Storage, traffic, and request count also matter.

---

## 1.5 AWS and web services

A web service is software available over the internet using standard request/response formats such as XML or JSON.

AWS is a platform of web services for:

- compute
- storage
- networking
- databases
- monitoring
- identity
- messaging
- deployment

AWS services are managed by API calls.

---

## 1.6 Ways to interact with AWS

All of these call AWS APIs underneath:

1. **Management Console**
   - GUI
   - good for manual setup and learning

2. **AWS CLI**
   - terminal commands
   - good for automation and repeated tasks

3. **SDK**
   - use AWS services from application code
   - language-specific wrappers around APIs

4. **Blueprints / IaC**
   - describe whole system as a template
   - examples: CloudFormation, Terraform

MCQ trap:

> Console, CLI, SDK, and CloudFormation are different interfaces to AWS APIs.

---

## 1.7 AWS global infrastructure

## Region

A Region is a geographical area.

Examples:

- Sydney
- London
- Tokyo
- Virginia

Each Region usually has multiple Availability Zones.

## Availability Zone

An AZ is an isolated part of a Region made of one or more data centres.

AZs are connected by low-latency private links.

Use multiple AZs for high availability.

## Edge location

Used for content delivery close to users.

Example:

- CloudFront edge location

---

## 1.8 Service scope

## Global services

Do not require choosing a Region/AZ.

Examples:

- IAM
- Route 53
- CloudFront

## Regional services

Created in a specific Region.

Examples:

- S3
- VPC
- RDS Multi-AZ
- CloudFormation
- Secrets Manager
- ALB
- ASG
- SQS
- SNS

## Zonal services

Belong to one AZ.

Examples:

- EC2
- EBS
- subnet
- single-AZ RDS

MCQ trap:

> VPC is regional, subnet is zonal, EBS is zonal.

---

## 1.9 Basic cloud architecture examples

## Simple web shop

Flow:

User → Internet → EC2 web server → RDS database

Use:

- EC2 for web server
- RDS for database

## Improved web shop

Flow:

User → Load Balancer → multiple EC2 instances → RDS  
Static content → S3 + CloudFront

Benefits:

- Load balancer distributes traffic
- multiple EC2s improve availability
- S3 stores static files
- CloudFront improves performance
- RDS reduces database maintenance

## Batch processing

Cloud is useful when jobs run only sometimes.

Example:

- daily analytics job
- start compute resources
- process data
- store results
- terminate compute

Benefit:

> Pay only while machines are actually used.

---

# 2. Week 2 — Cloud Storage and S3

## 2.1 Storage as SaaS vs IaaS/PaaS

## SaaS storage

End-user friendly cloud storage.

Examples:

- Google Drive
- OneDrive
- Dropbox
- iCloud

Features:

- file sharing
- collaboration
- versioning
- easy UI

## Infrastructure/platform storage

Used by applications and cloud systems.

Examples:

- AWS S3
- EBS
- EFS
- Google Cloud Storage
- Azure Storage

Less UI-focused but more controllable and cost-effective.

---

## 2.2 AWS storage services

## S3

Object storage.

- Access: API/HTTPS/CLI/SDK
- Max size: practically unlimited
- Latency: higher
- Cost: very low
- Best for: objects, backups, static files, data lakes

## EBS

Block storage attached to EC2 over network.

- Max volume in slide: 16 TiB
- Latency: low
- Cost: low
- Best for: EC2 disk, database disk, root volume

## EC2 Instance Store

Block storage physically attached to host.

- Very low latency
- Very low cost
- Temporary
- Data lost when instance stops/terminates
- Best for cache, buffer, scratch data

## EFS

File storage.

- NFSv4.1
- shared by multiple Linux instances
- medium latency/cost
- unlimited scale

MCQ shortcut:

- Object storage → S3
- Persistent EC2 disk → EBS
- Temporary fast local disk → Instance Store
- Shared Linux file system → EFS

---

## 2.3 File storage vs object storage

## File storage

Uses folder hierarchy:

`folder/subfolder/file.txt`

Good for:

- traditional file systems
- shared folders

## Object storage

Stores data as objects.

Each object has:

- unique key/identifier
- metadata
- content/data

Good for:

- massive scalability
- simple API access
- metadata
- distributed storage

MCQ trap:

> S3 is object storage, not a normal file system.

---

## 2.4 S3 basics

S3 = Simple Storage Service.

It is:

- one of AWS’s oldest services
- distributed object store
- accessed via HTTPS/API
- stores every uploaded file as an object

S3 object = data + metadata + key.

Bucket = container for objects.

Important:

- bucket name must be globally unique
- bucket is created in a Region
- S3 has flat structure
- folders are only key prefixes

---

## 2.5 S3 pseudo-folders

S3 does not have real folders.

Example object key:

`photos/2022/cat.jpg`

This appears as folders in console:

`photos` → `2022` → `cat.jpg`

But physically/logically it is one object key string.

A prefix query such as `photos/2022` returns all objects whose keys begin with that prefix.

MCQ trap:

> Objects in the same “folder” do not necessarily sit on the same physical server.

---

## 2.6 S3 object URL

Virtual-hosted style:

`https://bucket-name.s3-region.amazonaws.com/object-key`

Default region `us-east-1` can sometimes be omitted.

Deprecated older style:

`https://s3.region.amazonaws.com/bucket-name/object-key`

MCQ angle:

> Bucket name is part of the URL and must be globally unique.

---

## 2.7 S3 durability and redundancy

S3 redundantly stores data across multiple facilities/AZs within a Region.

Purpose:

- durability
- fault tolerance
- hardware failure protection

Important distinction:

## Redundancy

Protects against AWS hardware/data centre failure.

Default feature.

No extra cost as a basic S3 feature.

## Versioning

Protects against user overwrite/delete mistakes.

Must be enabled.

May increase storage cost.

MCQ trap:

> Redundancy is not the same as versioning.

---

## 2.8 S3 pricing

You pay for:

- GB stored per month
- PUT/COPY/POST/LIST/GET requests
- data transfer out of S3 Region
- retrieval charges for some storage classes

You usually do not pay for:

- data transfer into S3
- transfer to CloudFront or EC2 in same Region, according to the slide

MCQ trap:

> S3 cost includes requests and data transfer, not only storage.

---

## 2.9 S3 use cases

## Backup and archive

Store critical backups in S3.

Can replicate to another Region for disaster recovery.

## Media hosting

Store images/videos/audio.

Often use CloudFront to deliver faster.

## Static website hosting

Host HTML, CSS, JS, images.

Good:

- cheap
- scalable
- no server management

Limitation:

- static only, no backend logic by itself

## Data lake / analytics

Flow:

Raw data in S3 → compute processes it → processed data back to S3 → Athena/QuickSight analyses it.

## Event-driven processing

S3 can trigger events when objects change:

- object created
- object deleted
- object replicated

Can trigger:

- Lambda
- SNS
- SQS

Example:

Upload image → S3 event → Lambda creates thumbnail.

---

## 2.10 S3 storage classes

## S3 Standard

Use for frequently accessed data.

- high availability
- high durability
- higher storage cost

## S3 Intelligent-Tiering

Use when access pattern is unknown.

Automatically moves data between access tiers.

## S3 Standard-IA

Use for infrequently accessed data that still needs quick retrieval.

- lower storage cost
- retrieval charge
- minimum storage duration

## S3 One Zone-IA

Stored in one AZ.

Cheaper but less resilient.

Use for:

- reproducible data
- secondary copies

## Glacier Instant Retrieval

Archive data with instant retrieval.

## Glacier Flexible Retrieval

Archive data with slower retrieval.

## Glacier Deep Archive

Lowest-cost long-term archive.

Use for:

- compliance archive
- rarely accessed data

MCQ trap:

> Infrequent access but fast retrieval → Standard-IA.  
> Cheapest long-term archive → Glacier Deep Archive.

---

## 2.11 Lifecycle policy

Lifecycle policy = automatic rules for S3 objects.

Two main actions:

1. **Transition action**
   - move object to another storage class

2. **Expiration action**
   - delete object after a certain time

Example:

Standard for 60 days → Standard-IA for 1 year → Glacier for 7 years → delete.

Use:

- cost saving
- automatic data management
- no app code changes

---

## 2.12 S3 versioning

Versioning protects from accidental overwrite/delete.

## Upload same key

If versioning enabled:

- creates new version ID
- old version still exists

If versioning disabled:

- overwrites old object

## Delete without version ID

If versioning enabled:

- S3 adds delete marker
- old versions remain
- normal GET returns 404 if delete marker is latest

If versioning disabled:

- object deleted and cannot be retrieved

## GET with version ID

Retrieves that exact version, even if a delete marker is latest.

## Delete with version ID

Permanently deletes that specific version.

No delete marker is added.

MCQ trap:

> Delete marker hides the object in normal GET, but old versions still exist.

---

## 2.13 S3 consistency

S3 provides strong read-after-write consistency for:

- GET
- LIST
- PUT
- DELETE

Meaning:

After update/delete/write, later reads/lists see the latest result.

---

## 2.14 S3 replication

S3 replication automatically copies objects from source bucket to destination bucket.

Requires:

- versioning enabled on source bucket
- versioning enabled on destination bucket

Types:

- same-region replication
- cross-region replication
- cross-account replication
- multi-destination replication
- existing object replication

Reasons:

- disaster recovery
- compliance
- latency improvement
- cost optimisation

## Replication delete behaviour

If delete request has no version ID:

- source gets delete marker
- by default delete marker is not copied to destination
- can enable delete marker replication

If deleting a specific version:

- source deletes that version
- deletion is not replicated
- destination version must be manually deleted if needed

MCQ trap:

> Deleting source does not always delete destination.

---

# 3. Week 3 — Virtualisation and EC2

## 3.1 S3 review

S3 has flat structure.

Important:

- folder = 0-byte object
- delete marker = special object in versioned bucket
- object key determines access/organisation
- objects in same pseudo-folder may be stored on different physical machines

---

## 3.2 What is virtualisation?

Virtualisation = transparent emulation of an IT resource.

It makes a virtual/fake version of a resource look real to the user.

Examples:

- virtual memory
- virtual machine
- virtual IP
- virtual storage
- virtual network

Common features:

1. **Emulation**
   - real resource is represented virtually

2. **Transparency**
   - user/app cannot easily tell it is virtual

3. **Benefit**
   - resource sharing
   - scalability
   - high availability
   - isolation
   - better utilisation

---

## 3.3 Virtual memory

Virtual memory gives CPU/processes the illusion of more memory than physically available.

Actions involved:

- address translation
- data transfer
- data placement/swap strategy

Purpose:

- memory expansion
- code reuse
- process isolation

---

## 3.4 Mainframe virtualisation

IBM introduced mainframe virtualisation in 1972.

Purpose:

- run multiple OS/app environments on same mainframe
- support time-sharing
- solve OS migration issues caused by hardware changes

---

## 3.5 Hot Standby Router Protocol example

HSRP uses virtual IP address.

Purpose:

- avoid single point of failure in default gateway

How:

- multiple routers share a virtual IP
- router with highest priority is active
- backup router takes over if active fails

MCQ cue:

> Virtualisation can be used outside VMs, e.g., virtual IP for high availability.

---

## 3.6 Server virtualisation

A virtual machine is a software-defined computer running on a physical computer.

Physical computer = host.

Virtual machine = guest.

A host can run multiple guests.

Each VM has:

- OS
- virtual CPU
- virtual memory
- virtual disk
- virtual network

---

## 3.7 Hypervisor

Hypervisor manages VMs.

It:

- shares physical resources
- isolates VMs
- manages devices
- manages virtual storage
- controls hardware access

Goals:

1. provide environment identical to physical hardware
2. minimal performance cost
3. complete control of system resources

---

## 3.8 Type 1 vs Type 2 hypervisor

## Type 1 / bare-metal

Runs directly on hardware.

Used in cloud/data centre.

Better performance and isolation.

Examples:

- Xen
- Hyper-V
- KVM-style cloud setup

## Type 2 / hosted

Runs on top of a normal OS.

Used mostly for desktop/testing.

Examples:

- VirtualBox
- VMware Workstation

MCQ trap:

> Cloud providers usually use Type 1 style hypervisors.

---

## 3.9 Xen

Components:

## Xen Hypervisor

Thin software layer directly on hardware.

Manages:

- CPU
- memory
- interrupts

## Domain 0

Special privileged VM.

Handles:

- I/O
- management
- interaction with other VMs
- exposes control interface

## Guest domains

Normal user VMs.

## Toolstack/console

Admin interface to create/destroy/configure VMs.

MCQ trap:

> Domain 0 is not a normal customer VM.

---

## 3.10 Hyper-V

Components:

- hypervisor
- root partition
- child partitions

## Root partition

Similar to Xen Domain 0.

Handles:

- management
- I/O
- communication with other partitions

## Child partitions

Guest VMs.

---

## 3.11 KVM

KVM turns Linux kernel into a hypervisor.

A VM becomes like a Linux process, scheduled by the host OS, but executed in a special virtualisation mode.

Related tools:

## KVM

CPU/memory virtualisation.

## QEMU

Generic emulator for I/O devices.

## libvirt

Management layer for VMs across hypervisors.

MCQ trap:

> QEMU emulates devices; libvirt manages VMs.

---

## 3.12 CPU virtualisation

Physical CPU cores are shared by virtual CPUs.

vCPU = virtual CPU assigned to VM.

Hypervisor schedules vCPUs onto physical/logical CPUs.

Important:

- vCPU count = max number of threads VM can run at a moment
- VM can run on different physical CPUs over time
- multiple vCPUs require multiple physical/logical cores available
- too many vCPUs can increase scheduling wait
- many apps cannot use many threads

MCQ trap:

> More vCPUs does not always improve performance.

---

## 3.13 Memory virtualisation

Guest OS thinks it has its own memory.

Actually, hypervisor maps memory.

Two-level mapping:

guest virtual memory → guest physical memory → host physical memory

Purpose:

- isolation
- prevent one VM from accessing another VM’s memory
- allow hypervisor control

---

## 3.14 I/O virtualisation

I/O includes:

- disk
- network
- printer
- monitor
- mouse

Disk:

- VM sees virtual disk
- maps to real disk/storage

Network:

- VM sees virtual network adapter
- hypervisor/VMM handles access to physical NIC

---

## 3.15 EC2

EC2 provides resizable compute capacity.

EC2 instance = virtual machine in AWS.

Use EC2 when:

- need OS control
- need custom software
- need any workload
- need cost options
- need scalable compute

---

## 3.16 EC2 launch choices

When launching EC2, choose:

1. AMI
2. instance type
3. key pair
4. VPC/subnet/network settings
5. security group
6. storage
7. IAM role
8. user data
9. tags

---

## 3.17 AMI

AMI = Amazon Machine Image.

Template used to launch EC2.

Contains:

- OS
- software
- configuration

Benefits:

- repeatability
- reusability
- recoverability

You can:

- launch instance from AMI
- modify instance
- create new AMI from it
- copy AMI to other Regions

MCQ trap:

> AMI is a template, not a running server.

---

## 3.18 Instance type

Instance type determines:

- CPU/vCPU
- memory
- storage support
- network performance

Example:

`t3.large`

- `t` = family
- `3` = generation
- `large` = size

Categories:

- general purpose: balanced
- compute optimised: CPU-heavy
- memory optimised: RAM-heavy
- storage optimised: disk I/O-heavy
- accelerated computing: GPU/ML

MCQ cues:

- in-memory database → memory optimised
- machine learning/GPU → accelerated computing
- broad normal app → general purpose
- high CPU workload → compute optimised

---

## 3.19 Networking features

Network bandwidth depends on instance type.

To improve networking:

- use cluster placement group for interdependent instances
- enable enhanced networking

Enhanced networking types:

- ENA up to 100 Gbps
- Intel 82599 VF up to 10 Gbps

---

## 3.20 AWS Compute Optimizer

Recommends:

- instance type
- instance size
- Auto Scaling group configuration

Findings:

- under-provisioned
- over-provisioned
- optimised
- none

Use when optimising existing instances.

---

## 3.21 Key pair

Key pair has:

- public key stored by AWS
- private key file kept by user

Linux:

- use private key for SSH

Windows:

- use private key to decrypt admin password

MCQ trap:

> Private key stays with you; AWS stores public key.

---

## 3.22 EC2 network settings

Choose:

- VPC
- subnet
- public IP auto-assign or not

Public IP is needed for direct internet access.

Private subnet instances usually have no public IP.

---

## 3.23 Security group

Security group = firewall rules for EC2.

Rule defines:

- protocol
- port
- source/destination

Examples:

- SSH TCP 22 from your IP
- HTTP TCP 80 from anywhere
- HTTPS TCP 443 from anywhere

MCQ trap:

> Security group exists outside guest OS.

---

## 3.24 EC2 storage

Every EC2 has root volume.

Can attach data volumes.

## EBS

Persistent block storage.

Data remains after stop/start.

Can be encrypted.

Snapshots stored in S3.

Use for:

- OS/root disk
- database
- app data

## Instance store

Temporary local storage on host.

Lost when instance stops/terminates.

Use for:

- cache
- buffer
- scratch data

## EFS

Shared Linux file system.

## FSx for Windows

Shared Windows file system.

## S3

Object storage, not normal mounted block/file disk.

MCQ trap:

> Multiple Linux EC2s need shared file system → EFS, not EBS.

---

## 3.25 IAM role for EC2

Use IAM role if software inside EC2 needs AWS access.

Example:

EC2 app needs to read S3.

Attach role through instance profile.

Benefits:

- no hardcoded access keys
- temporary credentials
- safer access management

MCQ trap:

> EC2 uses instance profile to receive IAM role credentials.

---

## 3.26 User data

Startup script that runs first time instance starts.

Use for:

- install packages
- configure application
- download files
- start services

Benefit:

- fewer custom AMIs
- automation

---

## 3.27 Tags

Tags are key-value metadata.

Use for:

- filtering
- automation
- cost allocation
- access control

---

## 3.28 Nitro system

Nitro is AWS’s modern EC2 virtualisation system.

Earlier AWS used Xen.

Nitro uses:

- thin HVM-based hypervisor for CPU/memory
- Nitro cards for network, storage, management, monitoring, security
- security chip
- no Domain 0 needed

Benefits:

- better performance
- better isolation
- lower hypervisor overhead
- offloads storage/network tasks to hardware

Bare metal instances:

- still use Nitro cards
- no Nitro hypervisor
- selected only for special needs

MCQ trap:

> Nitro removes need for Domain 0 by moving management/storage/network to Nitro cards.

---

# 4. Week 4 — Cloud Database

## 4.1 Storage review

## S3

Object storage.

- unlimited scale
- high latency
- low cost

## EBS

Block storage.

- attached to EC2 through network
- low latency
- persistent

## Instance Store

Block storage.

- physically attached
- very low latency
- temporary

## EFS

File storage.

- shared by multiple instances
- medium latency/cost

---

## 4.2 Block vs object storage

If you change one character in a 1GB file:

## Block storage

Only the relevant block changes.

## Object storage

Whole object/file must be updated.

MCQ trap:

> Databases usually prefer block storage because small changes should not rewrite whole objects.

---

## 4.3 EBS

EBS = persistent storage attached to EC2 over network.

Important points:

- not part of EC2 instance
- can remain after EC2 terminates
- root volume delete-on-termination usually true by default
- extra data volumes delete-on-termination usually false by default
- can attach to one instance at a time normally
- replicated within its AZ
- snapshots stored in S3

MCQ trap:

> EBS is AZ-level. EC2 must be in same AZ to attach.

---

## 4.4 IP addresses and CIDR

IPv4 = 32-bit address.

Private IPv4 ranges:

- 10.0.0.0 – 10.255.255.255
- 172.16.0.0 – 172.31.255.255
- 192.168.0.0 – 192.168.255.255

CIDR format:

`IP/prefix`

Example:

`192.0.2.0/24`

Means first 24 bits fixed, remaining 8 bits flexible.

Number of addresses:

`2^(32-prefix)`

Example:

`/24` → `2^8 = 256`

MCQ trap:

> In AWS subnet, 5 IPs are reserved, so /24 gives 251 usable addresses.

---

## 4.5 VPC

VPC = logically isolated virtual network.

You control:

- private IP range/CIDR
- subnets
- route tables
- gateways
- security groups
- network ACLs

VPC:

- belongs to one Region
- can span multiple AZs
- dedicated to one AWS account
- isolated from other VPCs

---

## 4.6 Subnets

Subnet = segment of VPC IP range.

Rules:

- subnet CIDR is inside VPC CIDR
- subnet CIDRs cannot overlap
- subnet belongs to one AZ
- public subnet has route to internet gateway
- private subnet has no direct inbound internet route

AWS reserves 5 IP addresses per subnet.

---

## 4.7 Public and private IP

## Private IP

Used inside VPC.

Assigned from subnet CIDR.

## Public IP

Internet-routable.

Can be auto-assigned.

May change after stop/start.

## Elastic IP

Static public IP.

Can be reassigned to another instance.

---

## 4.8 Security groups

Security group = firewall for EC2/RDS/etc.

Default:

- deny all inbound
- allow all outbound

Important:

- allow rules only
- no explicit deny
- all rules evaluated before decision
- can attach multiple SGs to one instance
- one SG can attach to multiple instances
- stateful

Stateful means:

> If request traffic is allowed in, response traffic is automatically allowed out.

MCQ trap:

> Security group does not need separate return rule.

---

## 4.9 Running databases in cloud

## On-prem database

You manage everything:

- power
- rack
- server
- OS
- database install
- patches
- backups
- scaling
- HA

## Database on EC2

AWS manages physical infrastructure.

You manage:

- OS
- DB software
- patches
- backups
- scaling
- HA

Example:

MariaDB installed manually on EC2.

## Managed AWS database service

AWS manages:

- infrastructure
- OS
- DB installation
- patches
- backups
- HA

You manage:

- app optimisation
- DB users/permissions
- schema/data
- security settings

Example:

RDS.

---

## 4.10 Cloud-hosted vs cloud-native DB

## Cloud-hosted database

Existing DB engine hosted in cloud.

Examples:

- RDS MySQL
- RDS PostgreSQL
- MongoDB Atlas

## Cloud-native database

Designed specifically for cloud features.

Examples:

- DynamoDB
- Aurora
- Cosmos DB
- Bigtable
- Spanner

Cloud-native usually focuses on:

- scalability
- elasticity
- high availability
- distributed design

---

## 4.11 Database considerations

When choosing DB, ask:

- What throughput is needed?
- How much data? GB/TB/PB?
- What is the data model?
- What are the access patterns?
- Need low latency?
- Need high durability?
- Need high availability?
- Need regulatory compliance?
- Need vertical or horizontal scaling?

---

## 4.12 Relational vs non-relational

## Relational

Structure:

- tables
- rows
- columns
- strict schema

Benefits:

- SQL
- data integrity
- joins
- transactions
- structured data

Use cases:

- OLTP
- existing relational workload
- complex queries

Examples:

- RDS
- Aurora

## Non-relational

Structure:

- key-value
- document
- graph
- in-memory

Benefits:

- flexible schema
- high throughput
- high performance
- scalable

Use cases:

- JSON documents
- caching
- single-digit millisecond retrieval
- high read/write throughput

Examples:

- DynamoDB
- Neptune
- ElastiCache

MCQ trap:

> Complex joins → relational.  
> Caching → ElastiCache.  
> Key-value low-latency → DynamoDB.

---

## 4.13 Scaling

## Vertical scaling

Make one machine bigger.

More CPU/RAM.

Example:

Upgrade instance size.

## Horizontal scaling

Add more machines/replicas.

Example:

read replicas.

---

## 4.14 RDS

RDS = managed relational database service.

Supports engines:

- Aurora MySQL-compatible
- Aurora PostgreSQL-compatible
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server

RDS uses EBS volumes for database and log storage.

Benefits:

- less admin burden
- scalable
- backups and snapshots
- automatic host replacement
- VPC isolation
- security/compliance

---

## 4.15 RDS architecture

RDS instance is an isolated database environment.

It includes:

- managed compute instance
- DB engine
- EBS volumes for data/logs

One RDS instance can contain multiple user-created databases.

App EC2 connects to RDS on DB port.

Example:

- MySQL port 3306

Security group must allow app EC2/security group to connect to RDS on the DB port.

MCQ trap:

> EC2 → RDS requires SG network access plus DB credentials.

---

## 4.16 Multi-AZ RDS

Purpose:

- high availability
- durability
- automatic failover

How:

- primary DB in one AZ
- standby DB in another AZ
- synchronous replication

If primary fails:

- standby becomes primary

MCQ trap:

> Multi-AZ is mainly HA/failover, not read scaling.

---

## 4.17 RDS read replica

Purpose:

- read scaling
- offload read queries
- improve read-heavy performance
- possible disaster recovery

How:

- asynchronous replication
- read replica can be promoted to primary if needed
- can be in same or different AZ

MCQ trap:

> Read replica = performance/read scaling.  
> Multi-AZ = availability/failover.

---

## 4.18 RDS security best practices

Use:

- VPC for network control
- IAM policies for managing RDS resources
- security groups for connection access
- SSL/TLS for DB connections
- KMS encryption at rest
- DB engine users and permissions for data access

---

## 4.19 Aurora

Aurora = cloud-native relational DB compatible with MySQL/PostgreSQL.

Main design:

> Separate DB engine layer from storage layer.

Goal:

- full SQL support
- high scalability
- high durability
- fault tolerance
- high availability

Aurora is managed by RDS.

---

## 4.20 Aurora DB layer

From client view, similar to MySQL/PostgreSQL primary-replica setup.

Contains:

- primary writer
- up to 15 reader replicas
- modified MySQL/PostgreSQL engine

But actual data is not stored on the DB instance or EBS volume.

Data is in distributed cluster storage.

---

## 4.21 Aurora storage layer

Storage nodes:

- EC2 instances with local SSD
- spread across AZs
- persist and replicate data

Database volume is divided into fixed-size segments.

Slide says:

- segment size = 10GB
- each segment belongs to a Protection Group
- each PG has 6 copies
- 6 copies spread across 3 AZs
- 2 copies per AZ

---

## 4.22 Aurora durability and quorum

Replication factor = 6 copies.

Across = 3 AZs.

Write quorum = 4/6.

Read quorum = 3/6.

Can tolerate:

- losing one AZ without losing write ability
- losing one AZ + one extra node without losing read availability

MCQ trap:

> Aurora write is successful after 4 of 6 storage acknowledgements.

---

## 4.23 Aurora “log is the database”

Traditional DB write often writes:

- write-ahead log
- data page
- binary log
- double-write
- metadata

This causes write amplification.

Aurora reduces this by sending only redo logs.

During write:

1. primary sends redo logs to replicas and storage nodes
2. replicas update memory using redo logs
3. storage nodes persist redo logs
4. primary waits for 4/6 acknowledgements
5. storage nodes later materialise data pages in background

Benefit:

- less network I/O
- lower foreground write latency
- background processing handles heavy work

MCQ trap:

> Aurora sends redo logs, not full data pages.

---

## 4.24 Aurora storage node processing

Storage node steps:

1. receive log record
2. add to memory queue
3. persist record on disk
4. acknowledge primary
5. organise logs and detect gaps
6. gossip with peers to fill gaps
7. coalesce logs into data pages
8. stage logs/pages to S3
9. garbage collect old versions
10. validate CRC codes

Design goal:

> minimise foreground write latency by moving most work to background.

---

## 4.25 Aurora LSN concepts

## LSN

Log Sequence Number.

Monotonically increasing number for log records.

## CPL

Consistency Point LSN.

DB-level safe point, usually transaction boundary.

## VCL

Volume Complete LSN.

Storage-level highest LSN where all prior logs are available.

## VDL

Volume Durable LSN.

Highest CPL ≤ VCL.

Example:

- VCL = 1007
- CPLs = 900, 1000, 1100
- VDL = 1000

Because 1007 may be in middle of transaction.

MCQ trap:

> Recovery stops at safe completed transaction point, not random latest log.

---

# 5. Week 8 — CloudFormation and IaC

## 5.1 AWS services and API calls

AWS resources are created/managed by API calls.

Ways to send API calls:

- Management Console
- AWS CLI
- SDK
- CloudFormation/Blueprints

Example:

Creating EC2 calls:

`ec2:RunInstances`

MCQ trap:

> GUI and CLI both eventually call AWS APIs.

---

## 5.2 AWS CLI

AWS CLI = command-line interface for AWS.

Needs:

- installation
- configuration
- access key ID
- secret access key
- region

Example commands:

- `aws configure`
- `aws s3 ls`
- `aws ec2 describe-instances`
- `aws cloudformation create-stack`

Use CLI for:

- automation
- repeated tasks
- inspecting services
- uploading files
- creating infrastructure

---

## 5.3 Problem with manual console setup

Manual processes have risks:

- not repeatable at scale
- hard to deploy same setup in multiple Regions
- no version control
- hard rollback
- weak audit trail
- inconsistent configuration

MCQ cue:

> Manual console is easy but poor for repeatability and audit.

---

## 5.4 Infrastructure as Code

IaC = describe infrastructure in a template file.

Template is:

- human-readable
- machine-consumable

Benefits:

- repeatability
- reusability
- maintainability
- consistency
- version control
- easy cleanup by deleting stack
- easier updates by changing template

---

## 5.5 CloudFormation

CloudFormation models and manages AWS resources.

A collection of resources = stack.

Template = blueprint/class.

Stack = running object created from template.

CloudFormation can:

- create stack
- update stack
- delete stack
- detect drift

No extra charge for CloudFormation itself; pay for created resources.

MCQ trap:

> Deleting a stack deletes resources created by that stack unless protected by policies.

---

## 5.6 Stack grouping

Resources can be grouped by:

- cohesion
- lifecycle
- security
- layers

Common layered approach:

- network stack
- application stack
- database stack

---

## 5.7 YAML vs JSON

CloudFormation supports both YAML and JSON.

## YAML advantages

- readable
- less verbose
- no brackets
- comments supported
- quotes often optional

## JSON advantages

- common in APIs
- easier for systems to generate/parse

MCQ trap:

> YAML is easier for humans; JSON is more common for systems/APIs.

---

## 5.8 Template anatomy

Only required section:

- `Resources`

Optional:

- `AWSTemplateFormatVersion`
- `Description`
- `Metadata`
- `Parameters`
- `Rules`
- `Mappings`
- `Conditions`
- `Transform`
- `Outputs`

---

## 5.9 Resources section

Defines resources to create.

Each resource has:

- logical ID
- Type
- Properties

Type format:

`AWS::ServiceName::ResourceType`

Example:

`AWS::EC2::Instance`

MCQ trap:

> Logical ID is chosen by user; Type must follow AWS format.

---

## 5.10 Parameters

Parameters customise stack creation.

Example:

- key pair name
- instance type
- bucket name

Think:

- template = class
- parameters = constructor arguments
- stack = object

---

## 5.11 Outputs

Outputs return useful values after deployment.

Examples:

- instance ID
- public DNS
- bucket name
- load balancer endpoint

Can also export values for other stacks.

---

## 5.12 Intrinsic functions

## Ref

Returns value of parameter/resource.

Examples:

- parameter Ref → parameter value
- EC2 instance Ref → instance ID

## Fn::GetAtt / !GetAtt

Returns resource attribute.

Example:

`!GetAtt EC2Instance.PublicDnsName`

## Fn::Sub / !Sub

String substitution.

Useful for variables in strings.

## DependsOn

Specifies creation order.

Use when dependency is not automatically inferred.

MCQ trap:

> `Ref` return value depends on resource type.

---

## 5.13 Create/update stack with CLI

Create:

`aws cloudformation create-stack --stack-name mybucket --template-body file://my_bucket.yaml`

Update:

`aws cloudformation update-stack --stack-name mybucket --template-body file://my_bucket.yaml`

With parameter:

`--parameters ParameterKey=BucketName,ParameterValue=my-unique-bucket-name`

---

## 5.14 Infrastructure Composer

Visual way to inspect/build CloudFormation templates.

Helps view relationships between resources.

---

## 5.15 Typical template resources

A lab CloudFormation template may create:

- VPC
- public subnets
- private subnets
- route tables
- route table associations
- internet gateway
- security groups
- EC2 instance
- DB subnet group
- IAM role
- instance profile
- outputs

---

## 5.16 VPC with Internet Gateway

Resources:

- VPC
- InternetGateway
- VPCGatewayAttachment

`DependsOn` can ensure gateway attachment is created after VPC and IGW.

---

## 5.17 Route tables

Public route table:

- local VPC route
- `0.0.0.0/0` route to Internet Gateway

Private route table:

- local VPC route
- may have no internet route unless NAT exists

MCQ trap:

> Public subnet needs route to Internet Gateway.

---

## 5.18 Subnets in CloudFormation

Important subnet properties:

- `VpcId`
- `CidrBlock`
- `AvailabilityZone`
- `MapPublicIpOnLaunch`

Public subnet:

- `MapPublicIpOnLaunch: true`

Private subnet:

- usually no auto public IP

---

## 5.19 DB subnet group

DB subnet group is needed for RDS DB instance.

It groups private subnets where DB can be placed.

MCQ trap:

> RDS uses DB subnet group to know which subnets/AZs it can use.

---

## 5.20 Security groups in CloudFormation

App SG example:

- allow HTTP 80 from `0.0.0.0/0`

DB SG should allow:

- database port only from app SG

Not from everywhere.

---

## 5.21 EC2 UserData

UserData can be included in template.

`|` in YAML means multiline string.

`!Sub` allows variable substitution.

Use for:

- install packages
- download app files
- start web server

---

## 5.22 Debugging CloudFormation

Use Stack Events.

Stack Events show:

- which resource failed
- error message from AWS service
- creation/update order

Example issue:

Invalid security group in RDS.

Possible cause:

- CloudFormation returned security group name instead of ID
- wrong use of `Ref`
- VPC security group mismatch

MCQ trap:

> For CloudFormation failures, inspect stack events first.

---

# 6. Week 11 — Serverless Computing

## 6.1 Why serverless emerged

Early cloud users liked IaaS because it looked like physical servers.

But IaaS requires users to manage:

- VM
- OS
- runtime
- scaling
- patching
- availability

Serverless became attractive because users wanted simpler operation.

---

## 6.2 Function as a Service

FaaS means user writes function code, and cloud provider runs it when invoked.

Example:

Phone uploads image → cloud function creates thumbnail → stores result.

Good for:

- low duty-cycle workloads
- event-driven tasks
- short jobs
- unpredictable traffic
- background processing

Bad for:

- long-running processes
- stateful apps
- tightly coupled workflows
- fine-grained shared state

---

## 6.3 Critical serverless distinctions

Serverless has:

1. decoupled compute and storage
2. stateless computation
3. automatic resource provisioning
4. pay based on actual usage/execution, not allocated idle server

MCQ trap:

> Serverless does not mean no servers. It means user does not manage servers.

---

## 6.4 How serverless works

Flow:

Event/request → provider launches sandbox/container/microVM → function runs → function reads/writes external storage → response/event result

Function usually runs in limited-resource environment.

State is stored externally:

- S3
- DynamoDB
- RDS
- EFS
- other storage services

---

## 6.5 Benefits of serverless

## Provider benefits

- business growth
- more service usage
- better resource utilisation
- can use less popular spare machines

## Customer benefits

- higher programming productivity
- lower operations burden
- cost saving for intermittent workloads

---

## 6.6 Storage limitations

Most serverless functions do not have persistent local storage.

Problems:

- must fetch/store state externally
- extra latency
- extra request cost
- difficult fine-grained state sharing

S3:

- low storage cost
- higher access latency/cost

DynamoDB/key-value DB:

- high IOPS
- expensive
- scaling can take time

MCQ trap:

> Lambda should be stateless. Persistent state goes outside the function.

---

## 6.7 AWS serverless offerings

Compute:

- Lambda
- Lambda@Edge
- Fargate

Storage:

- S3
- EFS

Datastores:

- DynamoDB
- Aurora
- RDS Proxy

API:

- API Gateway

Integration:

- SNS
- SQS
- AppSync
- EventBridge

Orchestration:

- Step Functions

Analytics:

- Kinesis
- Athena

---

## 6.8 AWS Lambda

Lambda is fully managed compute.

Runs code:

- on schedule
- in response to events
- from S3/DynamoDB/etc.
- behind API Gateway
- near users with Lambda@Edge

Supports:

- Java
- Go
- PowerShell
- Node.js
- C#
- Python
- Ruby
- Runtime API

---

## 6.9 Lambda concepts

## Function

Code that Lambda runs.

## Trigger

Resource/configuration that invokes the function.

## Event

JSON document passed to function.

## Execution environment

Container/VM/microVM where function runs.

## Runtime

Language-specific environment.

## Deployment package

Code + dependencies.

Can be:

- zip
- container image

---

## 6.10 Lambda anatomy

Handler:

- entry point function

Event object:

- data sent during invocation

Context object:

- runtime info
- request ID
- log group
- remaining time
- function metadata

MCQ trap:

> Event = input data. Context = runtime metadata.

---

## 6.11 Lambda layers

Layer = shared code/dependencies/custom runtime.

Purpose:

- avoid repeating dependencies
- share libraries across functions
- make packages smaller
- cleaner deployment

---

## 6.12 Lambda local storage `/tmp`

Lambda has temporary local storage at `/tmp`.

Use for:

- downloading S3 files for processing
- intermediate temporary results

Important:

- ephemeral
- not permanent
- should not store durable state

---

## 6.13 Invoking Lambda

Ways:

- Lambda console testing
- AWS SDK
- Invoke API
- AWS CLI
- service trigger

CLI example:

`aws lambda invoke --function-name my-function ...`

---

## 6.14 Synchronous Lambda

Caller waits for result.

Examples:

- API Gateway → Lambda
- Lambda function URL

Flow:

Client → API Gateway/Lambda URL → Lambda → response

MCQ cue:

> Web/API request expecting immediate response = synchronous.

---

## 6.15 Asynchronous Lambda

Event is queued and processed later.

Examples:

- S3 event
- EventBridge scheduled event

Failures can go to:

- error destination
- dead-letter queue
- SQS error queue

MCQ cue:

> S3 upload triggering Lambda = asynchronous.

---

## 6.16 Lambda permissions

There are two different permission questions.

## Who can invoke Lambda?

Controlled by Lambda resource-based policy.

Examples:

- allow S3 to invoke
- allow API Gateway to invoke
- allow EventBridge to invoke
- allow SNS to invoke

## What can Lambda do when it runs?

Controlled by Lambda execution role.

Examples:

- `s3:GetObject`
- `s3:PutObject`
- `sqs:SendMessage`
- `logs:PutLogEvents`
- `secretsmanager:GetSecretValue`

MCQ trap:

> S3 invoking Lambda and Lambda reading S3 are different permissions.

---

## 6.17 S3 + Lambda example

Scenario:

User uploads file to S3 → S3 triggers Lambda → Lambda reads file → writes metadata to DB.

Need:

1. Lambda resource-based policy allows S3 to invoke Lambda.
2. Lambda execution role allows `s3:GetObject`.
3. Lambda execution role allows CloudWatch Logs.
4. RDS SG allows Lambda SG if Lambda accesses RDS inside VPC.

---

## 6.18 Multitenancy and isolation

Cloud workloads from different customers may share hardware.

Problems:

- security concern
- performance concern

Trusted workloads:

- same organisation
- lower isolation may be acceptable

Untrusted workloads:

- different customers
- malicious/buggy workloads possible
- strong isolation required

---

## 6.19 Early Lambda isolation

Early Lambda used:

- containers to isolate functions
- VMs to isolate user accounts

Functions from same account might run on same VM.

Functions from different accounts ran on different VMs.

Issue:

- inefficient resource usage
- at least one VM per account
- harder scheduling optimisation

---

## 6.20 MicroVM

MicroVM = lightweight VM.

Position:

- stronger isolation than container
- less overhead than traditional VM

Comparison:

| Type | Isolation | Resource use | Start latency | Use |
|---|---|---|---|---|
| Container | Medium | Low | Low | microservices |
| MicroVM | High | Medium | Medium | serverless |
| Traditional VM | High | High | High | general purpose |

AWS Lambda now uses Firecracker microVM.

MCQ trap:

> Firecracker is lightweight virtualisation for Lambda/serverless isolation.

---

## 6.21 Lambda location options

## Default Lambda

Runs in Lambda-managed VPC.

Can access:

- public internet
- S3
- DynamoDB
- Secrets Manager through AWS-managed endpoints

Cannot access:

- resources inside your private VPC, such as private RDS

## Lambda inside your VPC

Needed when Lambda must access:

- private RDS
- private EC2
- internal services

May need:

- subnets
- security groups
- VPC endpoints
- NAT depending on outbound internet/AWS service needs

MCQ trap:

> Lambda must be placed in your VPC to access private RDS.

---

## 6.22 Lambda design principles

Prefer:

- many small functions
- each function handles one event/task
- minimal permissions
- external workflow orchestration

Avoid:

- one giant Lambda
- function calling function chains
- recursive triggers
- long synchronous waiting

---

## 6.23 Lambda monolith anti-pattern

One Lambda contains all logic and handles all events.

Problems:

- hard to upgrade
- hard to maintain
- hard to reuse
- hard to test
- package too large
- hard to enforce least privilege

Better:

- split functions by route/event/purpose

---

## 6.24 Recursive Lambda anti-pattern

Example:

S3 upload triggers Lambda. Lambda writes object back into same bucket. That write triggers Lambda again.

Result:

- infinite loop
- runaway cost

Fix:

- use different destination bucket
- use prefix/suffix filters
- configure trigger carefully

MCQ trap:

> Same source and output resource can recursively invoke Lambda.

---

## 6.25 Lambda calling Lambda anti-pattern

Deep function call stack is bad.

Problems:

- multiple concurrent functions waiting
- unnecessary cost
- complex error handling
- slowest function limits workflow
- scaling complexity

Better:

- SQS
- Step Functions

---

## 6.26 Synchronous waiting inside one Lambda

Bad:

- one Lambda waits for multiple slow operations

Better:

- split into event-driven functions
- use queues/events
- use Step Functions for workflows

Example:

Lambda writes to S3 → S3 event triggers next Lambda → next Lambda writes DynamoDB.

---

## 6.27 API Gateway

API Gateway creates, publishes, maintains, monitors, and secures APIs.

It is an entry point to backend resources.

Can route to:

- Lambda
- EC2
- web apps
- VPC resources
- AWS services
- real-time apps

Supports:

- stages
- versions
- monitoring
- high concurrent API calls

---

## 6.28 API Gateway security

Can use:

- DDoS/injection protection
- resource policies
- authorization
- throttling

---

## 6.29 API Gateway architecture

Typical flow:

Client/mobile/web/on-prem → CloudFront → API Gateway → Lambda/EC2/VPC/AWS service

CloudWatch monitors.

API Gateway cache can improve performance.

---

## 6.30 API Gateway → Lambda vs ALB → EC2

## API Gateway → Lambda

- API Gateway invokes Lambda through AWS service API
- Lambda does not listen on a port
- Lambda scales automatically by invocations
- low idle cost
- permissions controlled by Lambda resource policy
- best for serverless APIs and short tasks

## ALB → EC2

- ALB forwards network traffic to instance and port
- EC2 web server must listen on port
- EC2 scales via Auto Scaling Group
- EC2 costs while running
- network controlled by security groups
- best for traditional web apps and long-running services

MCQ trap:

> ALB forwards HTTP traffic to EC2 port. API Gateway invokes Lambda.

---

# 7. Week 12 — Kubernetes

## 7.1 What is Kubernetes?

Kubernetes is a container orchestration platform.

It started from Google’s experience running containers internally with systems like Borg and Omega.

Kubernetes was introduced in 2014.

Meaning of name:

- Kubernetes = Greek word for pilot/helmsman

Main purpose:

> Run and manage containerised applications across a cluster of machines.

---

## 7.2 Why Kubernetes exists

Containers are useful, but large systems may have many containers/microservices.

Manual management becomes hard.

Kubernetes automates:

- deployment
- scheduling
- scaling
- restarting failed containers
- service discovery
- load balancing
- rolling changes
- cluster management

MCQ cue:

> Kubernetes is not just Docker. It manages containerised apps across a cluster.

---

## 7.3 Application running options

The slides compare:

1. traditional app on OS/kernel
2. containerised app sharing host kernel but isolated with libraries
3. virtual machine app with guest OS/kernel on hypervisor

Key idea:

- containers share host kernel
- VMs have separate guest OS
- Kubernetes manages containers at cluster scale

---

## 7.4 Infrastructure abstraction

Kubernetes hides the underlying hardware.

Developers/operators deploy through Kubernetes API.

Applications do not need to know which exact machine they run on.

Kubernetes treats many worker machines as one deployment platform.

MCQ trap:

> Kubernetes abstracts a cluster like an OS abstracts a computer.

---

## 7.5 Declarative deployment

Kubernetes uses declarative model.

You describe desired state.

Example desired state:

- run this image
- use 3 replicas
- expose port 80
- use these labels

Kubernetes makes actual state match desired state.

If something fails, Kubernetes tries to fix it.

MCQ cue:

> Declarative = say what you want, not step-by-step how to do it.

---

## 7.6 Automatic change management

If you change the application description:

- Kubernetes adds new components
- removes unwanted components
- restarts failed ones
- recreates missing ones

Example:

Desired replicas change from 2 to 4 → Kubernetes starts 2 more pods.

Desired replicas change from 4 to 2 → Kubernetes removes 2 pods.

---

## 7.7 Kubernetes and microservices

Monolith:

- one or few large applications
- can sometimes be managed manually

Microservices:

- many small services
- each service installed/configured/managed separately
- distributed nature requires automation

Kubernetes helps by:

- managing many microservices
- scaling each independently
- restarting failed parts
- exposing services

MCQ trap:

> Kubernetes is especially useful for distributed microservice systems.

---

## 7.8 Standardising the cloud

Kubernetes provides a standard API across providers.

Instead of using each provider’s proprietary deployment API, users can deploy apps with Kubernetes API on:

- AWS
- Google Cloud
- Microsoft Azure
- IBM Cloud
- on-prem clusters

Benefit:

- portability
- less vendor-specific deployment logic

MCQ cue:

> Kubernetes standardises application deployment across cloud providers.

---

## 7.9 Kubernetes as OS of a cluster

A normal OS sits between applications and one computer’s hardware.

Kubernetes sits between distributed applications and a cluster of machines.

Analogy:

- OS manages CPU/RAM/disk for one computer
- Kubernetes manages nodes/pods/services for a cluster

MCQ trap:

> Kubernetes is like an operating system for a computer cluster.

---

## 7.10 Main Kubernetes services

Kubernetes provides:

## Service discovery

Allows applications to find other applications/services.

Example:

Frontend finds backend service without knowing exact pod IP.

## Horizontal scaling

Replicates application to handle load.

Example:

increase replicas from 2 to 5.

## Load balancing

Distributes traffic across replicas.

## Self-healing

Automatically restarts failed applications and moves them to healthy nodes if node fails.

## Leader election

Chooses one active instance while others wait as backup.

Use when only one instance should be active at a time.

---

## 7.11 Cluster structure

Kubernetes cluster has two planes:

## Control plane

Brain of the cluster.

Runs cluster management components.

Usually on master nodes.

## Workload plane

Runs applications.

Usually on worker nodes.

MCQ trap:

> Control plane manages the cluster; worker nodes run the application workload.

---

## 7.12 Control plane components

## API Server

Central communication point.

Users, operators, and Kubernetes components communicate through it.

Provides RESTful Kubernetes API.

## Scheduler

Decides which worker node should run each pod/application instance.

## Controller Manager

Runs controllers that maintain desired state.

Examples:

- replica management
- node tracking
- handling node failures

## etcd

Reliable distributed key-value store.

Stores cluster configuration and state.

MCQ trap:

> etcd stores cluster state; API Server is the central API entry point.

---

## 7.13 Worker node components

Worker node can be physical or virtual machine.

It runs application pods.

Main components:

## Kubelet

Agent on each worker node.

Talks to API Server.

Manages containers/pods on that node.

Uses cgroups and traffic control to reserve/manage resources.

## Kube-proxy

Maintains network rules on nodes.

Helps service networking/load balancing.

## Container runtime

Runs containers.

Examples:

- Docker
- containerd
- other runtimes

MCQ trap:

> Kubelet manages pods/containers on a node; kube-proxy manages network rules.

---

## 7.14 Running an application in Kubernetes

Basic steps:

1. Package application into one or more container images.
2. Push images to image registry.
3. Create Kubernetes objects representing the application.
4. Use command or manifest file to submit to Kubernetes.

Example command:

`kubectl create deployment kiada --image=luksa/kiada:0.1`

Flow:

1. user runs kubectl command
2. kubectl sends request to Kubernetes API
3. Deployment object created
4. Kubernetes creates Pod object based on deployment
5. pod assigned to worker node
6. kubelet tells container runtime to pull image and run container
7. app runs in container inside pod

MCQ trap:

> kubectl talks to API Server, not directly to Docker on worker node.

---

## 7.15 Kubernetes objects

Kubernetes objects are entities created and managed by the cluster.

Common objects:

- Pod
- Deployment
- Service
- Namespace
- etc.

They represent what is needed to run an application.

---

## 7.16 Three basic objects for simple app

A simple Kubernetes app usually uses:

1. **Deployment**
   - manages replicas of application

2. **Pod**
   - each application instance runs in a pod

3. **Service**
   - exposes pods as stable network endpoint

Flow:

Deployment → creates/manages Pods → Service exposes Pods.

MCQ trap:

> Deployment manages pod replicas; Service exposes pods.

---

## 7.17 Pod

Pod is the unit of scheduling in Kubernetes.

A pod is a resource envelope where one or more containers run.

Important:

- one pod is scheduled onto one node
- pod never spans multiple nodes
- one node can run many pods
- containers in same pod are scheduled together
- containers in same pod can share local volumes and network namespace

MCQ trap:

> Smallest deployable/schedulable unit in Kubernetes = Pod, not container.

---

## 7.18 Node, pod, container relationship

Node:

- physical or virtual machine

Pod:

- logical group of one or more containers
- scheduled onto a node

Container:

- actual running application process/image environment

Relationship:

Cluster → Nodes → Pods → Containers

---

## 7.19 Why pods are needed

Containers are designed to run one main process.

Running many independent processes in one container is an anti-pattern because:

- processes may need different resources
- hard to restart one process independently
- hard to monitor separately
- reduces containerisation benefits

But some processes are closely related and should run together.

Pod solves this by grouping related containers while keeping them separate containers.

---

## 7.20 Splitting app into pods

Bad:

- frontend and backend in same container
- frontend and backend in same pod if loosely related

Better:

- frontend pod
- backend pod

Reason:

- scale independently
- manage independently
- restart independently
- avoid unnecessary coupling

Example:

Stateless frontend can have multiple replicas; stateful backend may scale differently.

MCQ trap:

> Loosely related components should be separate pods.

---

## 7.21 Containers inside same pod

Normal containers:

- each has own namespace
- own IP address

Containers in same pod:

- share same network namespace
- share same pod IP address
- share port space
- communicate through localhost/loopback
- cannot bind to same port

Example:

Container 1 uses port 80, Container 2 cannot also bind port 80 in same pod.

MCQ trap:

> Containers in same pod share IP and port space.

---

## 7.22 Sidecar container

Sidecar = helper container in same pod as primary container.

Use only when helper complements primary process.

Examples:

- log collector beside web server
- content updater beside web server
- proxy beside app container

Primary container:

- main application

Sidecar:

- supports primary container

MCQ trap:

> Sidecar should complement primary container, not be a separate independent service.

---

## 7.23 Deployment

Deployment represents an application/microservice deployed in cluster.

In microservices:

- one Deployment commonly represents one microservice

Deployment specifies desired state:

- container image
- pod template
- node requirements
- number of replicas
- resource requirements

Deployment manages:

- creating pods
- keeping desired replica count
- replacing failed pods
- rolling changes

MCQ trap:

> Deployment is declarative controller for replicated pods.

---

## 7.24 Service

Pods are ephemeral:

- pod IPs are private
- pod IPs can change
- pods may be replaced on other nodes

Service provides stable address for a group of pods.

Use Service when:

- replicas need load balancing
- external clients need access
- other pods need stable endpoint

Service selects pods usually using labels.

MCQ trap:

> Service gives stable access to changing pods.

---

## 7.25 Kubernetes Service vs Deployment

Deployment:

- manages pods/replicas
- controls desired number of application instances

Service:

- exposes pods
- load balances traffic to pods
- gives stable endpoint

MCQ cue:

> “Keep 3 copies running” = Deployment.  
> “Give stable IP/name to reach them” = Service.

---

## 7.26 ECS vs Kubernetes comparison

| Concept | ECS | Kubernetes |
|---|---|---|
| Unit of deployment | Task | Pod |
| Long-running replicated component | ECS Service | Deployment |
| Container definition | Task definition | Pod template |
| Cluster machines | EC2 container instances/Fargate | Worker nodes |
| Load balancing | ALB/NLB target group | Service |
| Desired count | Service desired tasks | Deployment replicas |

MCQ trap:

> ECS Task ≈ Kubernetes Pod.

---

## 7.27 Namespace

Namespace groups and isolates objects/resources inside one cluster.

Uses:

- avoid naming conflicts
- separate environments
- organise apps/users

Example:

- `production` namespace has web/api/worker deployments
- `dev` namespace can also have web/api/worker deployments

Same object names can exist in different namespaces.

MCQ trap:

> Namespace isolates object names/resources within a cluster.

---

## 7.28 Labels

Labels are key-value pairs attached to objects.

Used for:

- identifying objects
- grouping objects
- selecting pods for services/deployments

Like AWS tags.

Examples:

- `app=nginx`
- `env=prod`
- `creation_method=manual`

MCQ trap:

> Service usually uses labels/selectors to find pods.

---

## 7.29 Annotations

Annotations are also key-value pairs.

Used for:

- descriptions
- extra metadata
- tool information

Difference from labels:

- labels identify/select objects
- annotations store descriptive/non-identifying metadata

MCQ trap:

> Labels are for selection/grouping; annotations are for extra metadata.

---

## 7.30 Manifest file

Kubernetes object can be described in YAML or JSON manifest.

Required/common fields:

## apiVersion

Which Kubernetes API version to use.

Example:

- `v1`
- `apps/v1`

## kind

Object type.

Examples:

- Pod
- Deployment
- Service

## metadata

Identity info.

Includes:

- name
- UID
- namespace
- labels
- annotations

## spec

Desired state.

Examples:

- image
- replicas
- ports
- selectors

MCQ trap:

> `spec` describes desired state.

---

## 7.31 Pod manifest

Pod manifest describes:

- API version
- kind: Pod
- pod name
- container image
- container name
- container port

Standalone pod manifests are mostly used for short tests.

In real apps, pods are usually created through Deployments.

MCQ trap:

> Standalone Pod is less common for production; Deployment is used to manage pods.

---

## 7.32 Deployment manifest

Example fields:

- `apiVersion: apps/v1`
- `kind: Deployment`
- `metadata.name`
- `spec.replicas`
- `spec.selector.matchLabels`
- `spec.template`
- `template.metadata.labels`
- `template.spec.containers`
- container image
- container port

Important:

- selector decides which pods belong to Deployment
- template describes pods to create
- replicas says desired pod count

MCQ trap:

> Deployment contains a pod template.

---

## 7.33 Service manifest

Service manifest includes:

- kind: Service
- port service exposes
- targetPort container receives traffic on
- selector to choose pods

Example logic:

- Service port: 80
- targetPort: 8080
- selector: `app=kubia`

This means:

> traffic to service port 80 forwards to port 8080 of pods labelled `app=kubia`.

MCQ trap:

> Service uses selector labels to choose backend pods.

---

# 8. Final Combined MCQ Memory List

## 8.1 Service selection

- Cloud software app → SaaS
- Managed app platform → PaaS
- VM/network/storage control → IaaS
- Virtual machine → EC2
- Object storage → S3
- Persistent EC2 block disk → EBS
- Temporary fast local EC2 disk → Instance Store
- Shared Linux file system → EFS
- Managed relational database → RDS
- Cloud-native relational database → Aurora
- Key-value NoSQL → DynamoDB
- Infrastructure template → CloudFormation
- Event-driven function → Lambda
- API entry point for Lambda → API Gateway
- Traditional web app endpoint → ALB + EC2
- Container cluster orchestration → Kubernetes

---

## 8.2 Scope memory

Global:

- IAM
- Route 53
- CloudFront

Regional:

- S3
- VPC
- RDS Multi-AZ
- CloudFormation
- Secrets Manager
- ALB
- ASG
- SNS
- SQS

Zonal:

- EC2
- EBS
- subnet
- single-AZ RDS

---

## 8.3 Storage traps

- S3 = object storage, not file system
- S3 folders = prefixes/pseudo-folders
- S3 bucket name = globally unique
- S3 bucket = regional
- EBS = block storage, AZ-level
- Instance store = temporary
- EFS = shared file system for Linux
- S3 redundancy protects hardware failure
- S3 versioning protects accidental overwrite/delete
- S3 replication needs versioning on both buckets
- Delete markers are not replicated by default
- deleting a specific version is not replicated

---

## 8.4 EC2 traps

- AMI = template
- Instance type = CPU/RAM/network/storage capability
- Key pair = public key in AWS, private key with user
- User data = first boot script
- IAM role = app on EC2 accesses AWS services
- Instance profile connects EC2 to IAM role
- Security group is outside guest OS
- Public IP can change after stop/start
- Elastic IP is static public IP
- Nitro = thin hypervisor + Nitro cards
- Too many vCPUs can hurt scheduling

---

## 8.5 Database traps

- RDS uses EBS
- RDS Multi-AZ = high availability/failover
- RDS read replica = read scaling
- Multi-AZ uses synchronous replication
- read replica uses asynchronous replication
- Aurora separates DB engine and storage
- Aurora has 6 storage copies across 3 AZs
- Aurora write quorum = 4/6
- Aurora read quorum = 3/6
- Aurora sends redo logs, not full data pages
- VDL = highest CPL ≤ VCL

---

## 8.6 CloudFormation traps

- CloudFormation template creates stack
- Stack = collection of resources
- Template = reusable blueprint/class
- Resources section is required
- Parameters customise creation
- Outputs return useful values
- Ref return depends on resource type
- GetAtt returns resource attribute
- DependsOn controls creation order
- Stack Events are used for debugging
- Manual console lacks repeatability/audit/version control

---

## 8.7 Lambda/serverless traps

- Serverless still uses servers; user does not manage them
- Lambda functions should be stateless
- persistent state goes to S3/DynamoDB/RDS/etc.
- `/tmp` is temporary
- Event object = invocation input
- Context object = runtime metadata
- Layers share dependencies
- Synchronous = caller waits
- Asynchronous = event queued
- S3 trigger = async
- API Gateway request = usually sync
- Lambda execution role = what Lambda can do
- Lambda resource policy = who can invoke Lambda
- Firecracker = MicroVM for Lambda isolation
- Lambda in default service VPC cannot access private RDS
- Lambda monolith is bad
- recursive S3→Lambda→same S3 can cause runaway loop
- Step Functions/SQS better than Lambda-calling-Lambda chains

---

## 8.8 API Gateway vs ALB

API Gateway → Lambda:

- invokes function
- no backend port
- scales by Lambda concurrency
- low idle cost
- controlled by Lambda resource policy

ALB → EC2:

- forwards HTTP traffic
- backend listens on port
- scales using ASG
- EC2 costs while running
- controlled by security groups

---

## 8.9 Kubernetes traps

- Kubernetes = container orchestration platform
- Kubernetes is like OS for a cluster
- Control plane = brain
- Worker nodes = run apps
- API Server = central communication/API entry
- Scheduler = chooses worker node
- Controller Manager = maintains desired state
- etcd = stores cluster state
- Kubelet = manages pods/containers on worker node
- Kube-proxy = network rules/service routing
- Container runtime = runs containers
- Pod = smallest schedulable unit
- Pod can contain one or more containers
- Pod never spans multiple nodes
- One node can run many pods
- Containers in same pod share IP/port namespace
- Sidecar = helper container for primary container
- Deployment = manages replicated pods
- Service = stable endpoint/load balancing for pods
- Namespace = object/resource isolation
- Labels = selection/grouping
- Annotations = descriptive metadata
- Manifest = YAML/JSON desired state
- `apiVersion`, `kind`, `metadata`, `spec` are key manifest fields
- ECS Task ≈ Kubernetes Pod
- ECS Service ≈ Kubernetes Deployment
- ECS Task Definition ≈ Kubernetes Pod template

---

# 9. How MCQs may ask these topics

## Question type: choose correct service

Example:

A company wants to store static images cheaply and serve them through HTTPS.

Correct thinking:

- static object data
- cheap
- scalable
- HTTPS API

Answer: S3.

---

## Question type: identify wrong statement

Example:

Which statement is false?

- S3 bucket names must be globally unique.
- S3 folders are real directories.
- S3 supports versioning.
- S3 can trigger Lambda.

False: S3 folders are real directories.

---

## Question type: HA vs scaling

Example:

A database must survive AZ failure with automatic failover.

Answer: RDS Multi-AZ.

A database has too many read queries.

Answer: Read replica.

---

## Question type: permission

Example:

S3 upload should trigger Lambda. What permission is needed?

Answer:

- Lambda resource-based policy allows `s3.amazonaws.com` to invoke Lambda.

If Lambda then reads S3 object:

- Lambda execution role needs `s3:GetObject`.

---

## Question type: Kubernetes object

Example:

Which object gives stable network access to changing pods?

Answer: Service.

Which object manages desired number of pod replicas?

Answer: Deployment.

Which object is smallest schedulable unit?

Answer: Pod.

---

## Question type: CloudFormation

Example:

Which section is required?

Answer: Resources.

Which function returns public DNS?

Answer: `Fn::GetAtt` / `!GetAtt`.

Which tool helps debug stack creation failure?

Answer: Stack Events.

---

## Question type: Lambda anti-pattern

Example:

S3 triggers Lambda, Lambda writes back to same bucket, causing repeated invocations.

Answer: recursive/runaway Lambda anti-pattern.

---

# 10. Last-Minute 40/40 MCQ Checklist

Memorise these exact pairs:

- S3 = object storage
- EBS = persistent block storage
- Instance Store = temporary local block storage
- EFS = shared Linux file system
- EC2 = virtual machine
- AMI = EC2 template
- Security group = stateful firewall
- NACL = stateless subnet firewall
- VPC = regional
- Subnet = AZ-level
- EBS = AZ-level
- IAM = global
- CloudFormation = IaC stack
- RDS Multi-AZ = HA/failover
- RDS Read Replica = read scaling
- Aurora = SQL + distributed cloud storage
- Aurora write = 4/6 quorum
- Aurora read = 3/6 quorum
- Lambda = stateless function
- Lambda execution role = what Lambda can do
- Lambda resource policy = who can invoke Lambda
- API Gateway = API front door
- ALB = HTTP traffic distribution to EC2/container targets
- Kubernetes = cluster OS/container orchestration
- Pod = scheduling unit
- Deployment = desired replicas
- Service = stable endpoint
- Namespace = isolation/grouping
- Label = selection
- Annotation = metadata
