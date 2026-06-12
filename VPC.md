# COMP5349 VPC Connection Questions — Practical Exam Guide

## 0. The one rule to remember
A network connection works only when **every required layer allows it**:

**Source app → source security group outbound → source subnet route table → gateway/local route → destination subnet route table/NACL → destination security group inbound → destination app/service listens on that port**

For most exam questions, do not start by memorising services. Start by tracing the packet.

---

## 1. The 6 questions to ask for any VPC connection

### Q1. Who is talking to whom?
Identify:
- **Source**: Internet user, ALB, bastion, web EC2, Lambda, RDS, S3, etc.
- **Destination**: ALB, web server, RDS, internet, S3, another EC2, etc.
- **Protocol + port**: HTTP 80, HTTPS 443, SSH 22, MySQL 3306, app 8080.
- **Direction**: request direction matters. Web → DB is different from DB → Web.

Exam wording trap: “Can A connect to B?” means check the path from **A to B**, not just whether B exists.

---

### Q2. Are source and destination inside the same VPC CIDR?
If both private IPs are inside the same VPC CIDR, e.g. VPC `10.0.0.0/16`, then the route is usually:

```text
Destination: 10.0.0.0/16 → Target: local
```

That means subnets inside the VPC can route to each other by private IP, even if one subnet is public and another is private.

Important: the **local route only solves routing**. Security groups and NACLs can still block the connection.

---

### Q3. Is the destination the public internet?
If an EC2 instance or service needs to send traffic to the internet:

#### Public subnet outbound to internet
Needs:
```text
Subnet route table: 0.0.0.0/0 → Internet Gateway
Resource has public IPv4 / Elastic IP, or it is an internet-facing ALB
Security group outbound allows it
NACL allows request and response
```

#### Private subnet outbound to internet
Needs:
```text
Private subnet route table: 0.0.0.0/0 → NAT Gateway
NAT Gateway is in a public subnet
NAT Gateway has Elastic IP
Public subnet route table: 0.0.0.0/0 → Internet Gateway
Security group outbound allows it
NACL allows request and response
```

NAT Gateway is for **outbound from private subnet**, not inbound from internet.

---

### Q4. Is the source the public internet?
For the internet to initiate traffic into AWS:

The destination must be internet-reachable:
```text
Public subnet route table has 0.0.0.0/0 → Internet Gateway
Destination has a public IP, Elastic IP, or is an internet-facing ALB
Destination security group inbound allows the port from the internet
NACL allows both directions
Application listens on the port
```

Private subnet resources should normally **not** be directly reachable from the internet.

---

### Q5. What do the security groups allow?
Security groups are resource-level firewalls. They are **stateful**, so if inbound request traffic is allowed, the return traffic is automatically allowed.

Check two things:

```text
Destination SG inbound: allows source on correct port?
Source SG outbound: allows destination on correct port?
```

Most AWS default/custom SGs allow all outbound, but exams may specify restricted outbound, so always check.

Best exam pattern:
```text
ALB SG: inbound 80/443 from 0.0.0.0/0
Web SG: inbound 80/443 from ALB SG only
DB SG: inbound 3306 from Web SG only
Bastion SG: inbound 22 from your IP only, not 0.0.0.0/0 ideally
```

Security group reference means “allow traffic from any resource attached to that SG”, not from the SG object itself.

---

### Q6. Are Network ACLs mentioned?
If the question does not mention custom NACLs, assume default NACL usually allows all.

If NACLs are mentioned, remember:

```text
NACL = subnet-level firewall
NACL = stateless
Need inbound rule + outbound rule
Can allow and deny
Rules are evaluated by rule number order
```

Security groups are usually the main exam focus unless NACL rules are explicitly listed.

---

## 2. Public subnet vs private subnet — practical meaning

### Public subnet
A subnet is public if its route table has:
```text
0.0.0.0/0 → Internet Gateway
```

But for an EC2 instance to actually be reachable from the internet, it also needs a public IP/EIP and SG inbound permission.

### Private subnet
A subnet is private if it does not route internet-bound traffic directly to an IGW.

Common private subnet route:
```text
10.0.0.0/16 → local
0.0.0.0/0 → NAT Gateway
```

This gives outbound internet access but blocks inbound internet initiation.

### Isolated/private DB subnet
A DB subnet often has only:
```text
10.0.0.0/16 → local
```

This means the DB can talk inside the VPC but has no internet route.

---

## 3. The route table decision process

For every outgoing packet, ask:

1. What is the destination IP?
2. Which route table is associated with the **source subnet**?
3. Which route matches the destination?
4. If more than one route matches, choose the most specific route.
5. Where does the target send it?

Example:
```text
VPC: 10.0.0.0/16
Private EC2 wants 10.0.3.25 RDS
Route match: 10.0.0.0/16 → local
So traffic stays inside VPC.
```

Example:
```text
Private EC2 wants 8.8.8.8
Route match: 0.0.0.0/0 → NAT Gateway
So traffic goes to NAT, then IGW, then internet.
```

---

## 4. How to solve the mock-style VPC architecture question

Given architecture:

```text
VPC: 10.0.0.0/16
Public Subnet: 10.0.1.0/24, RT-Public, ALB, NAT Gateway, Bastion
App Subnet: 10.0.2.0/24, RT-App, Web EC2
DB Subnet: 10.0.3.0/24, Default RT, RDS MySQL

RT-Public:
10.0.0.0/16 → local
0.0.0.0/0 → Internet Gateway

RT-App:
10.0.0.0/16 → local
0.0.0.0/0 → NAT Gateway

Default Route Table:
10.0.0.0/16 → local

Security Groups:
ALB SG inbound TCP 80 from 0.0.0.0/0
Bastion SG inbound TCP 22 from 0.0.0.0/0
Web SG inbound TCP 80 from ALB SG
RDS SG inbound TCP 3306 from Web SG
```

### A. Internet user → ALB on HTTP 80
Works if ALB is internet-facing and in the public subnet.

Why:
```text
Internet → IGW → public subnet → ALB
RT-Public has 0.0.0.0/0 → IGW
ALB SG allows TCP 80 from 0.0.0.0/0
```

### B. ALB → Web servers on HTTP 80
Works.

Why:
```text
ALB private IP → Web EC2 private IP
Both are inside 10.0.0.0/16, so local route handles routing
Web SG allows TCP 80 from ALB SG
Security groups are stateful, so response is allowed
```

Important: Web servers do **not** need public IPs when they are behind the ALB.

### C. Web servers → RDS MySQL on 3306
Works only if RDS SG allows TCP 3306 from Web SG.

Why:
```text
Web EC2 10.0.2.x → RDS 10.0.3.x
Both are inside VPC, so route is local
RDS SG must allow MySQL 3306 from Web SG
DB subnet does not need internet route
```

### D. Internet user → Web EC2 directly
Does not work.

Why:
```text
Web servers are in App subnet
App route table sends internet-bound traffic to NAT, not IGW
Web SG only allows TCP 80 from ALB SG, not 0.0.0.0/0
```

Even if the web app is running, the intended path is Internet → ALB → Web, not Internet → Web.

### E. Web servers → internet for updates/packages
Works if NAT Gateway is correctly configured.

Needed:
```text
Web/App subnet RT: 0.0.0.0/0 → NAT Gateway
NAT Gateway is in public subnet
NAT Gateway has Elastic IP
Public subnet RT: 0.0.0.0/0 → IGW
Web SG outbound allows traffic
```

### F. Internet → RDS directly
Does not work and should not work.

Why:
```text
RDS is in DB subnet
DB subnet has only local route
RDS SG should allow only Web SG on 3306
No direct public internet path
```

### G. Bastion → Web EC2 via SSH 22
Not enough information / likely blocked if Web SG only has inbound TCP 80 from ALB SG.

To allow it, add:
```text
Web SG inbound TCP 22 from Bastion SG
```

The bastion itself is reachable from internet because it is in the public subnet and has SSH inbound, but that does not automatically give it access to the web instances.

### H. Bastion → RDS directly
Usually should be blocked unless explicitly allowed.

To allow it, RDS SG would need:
```text
Inbound TCP 3306 from Bastion SG
```

But best practice is usually app-only DB access, not broad bastion DB access unless required for admin.

### I. RDS → internet
No, based on the given default route table.

Why:
```text
DB subnet route table only has 10.0.0.0/16 → local
No 0.0.0.0/0 route to NAT or IGW
```

---

## 5. Minimal fix questions — how to answer

When exam asks “what should be changed?”, give the smallest safe change.

### Problem: Web server cannot access internet
Check/fix:
```text
App subnet route table must have 0.0.0.0/0 → NAT Gateway
NAT Gateway must be in public subnet
Public subnet route table must have 0.0.0.0/0 → IGW
NAT Gateway must have Elastic IP
Web SG outbound must allow required traffic
```

### Problem: User cannot access website
Check/fix:
```text
ALB must be internet-facing
ALB must be in public subnets
Public subnet route table must have 0.0.0.0/0 → IGW
ALB SG inbound must allow 80/443 from user IP or 0.0.0.0/0
ALB listener must forward to target group
Target group health checks must pass
Web SG inbound must allow traffic from ALB SG
```

### Problem: ALB target is unhealthy
Check/fix:
```text
Web server app running?
Correct target group port?
Correct health check path?
Web SG allows health check traffic from ALB SG?
NACL allows traffic both ways?
Instance route back to ALB works through local VPC route?
```

### Problem: Web cannot connect to RDS
Check/fix:
```text
RDS SG inbound TCP 3306 from Web SG
Web SG outbound allows TCP 3306 to RDS SG
RDS is listening on 3306
Correct database endpoint, username, password
NACL allows both directions
RDS subnet group exists in correct private subnets
```

### Problem: SSH to private web instance fails through bastion
Check/fix:
```text
Bastion reachable on 22 from your IP
Bastion has public IP/EIP
Bastion is in public subnet with IGW route
Web SG inbound TCP 22 from Bastion SG
Correct private key and username
NACL allows SSH path
```

---

## 6. Common exam traps

### Trap 1: “Public subnet” alone is not enough
For EC2 internet access, you also need a public IP/EIP and SG/NACL permission.

### Trap 2: NAT Gateway does not allow inbound internet traffic
NAT Gateway lets private resources initiate outbound traffic. Internet users cannot use NAT to start a connection to private EC2.

### Trap 3: Route table and security group solve different problems
Route table answers: “where should packet go next?”
Security group answers: “is this resource allowed to receive/send this port?”

### Trap 4: Local route does not mean all ports are open
Local route allows private routing inside the VPC. SG/NACL still control ports.

### Trap 5: RDS does not need public subnet for app access
If EC2 app and RDS are in the same VPC, app can reach RDS using the local route and SG rule.

### Trap 6: SGs are stateful, NACLs are stateless
For SG, return traffic is automatic.
For NACL, allow both request and response directions.

### Trap 7: SG source can be another SG
This is preferred because it follows dynamic membership. Example: DB SG allows `3306` from `WebSG` instead of hardcoding web instance IPs.

### Trap 8: Default route table with only local route means no internet
A subnet associated with only:
```text
10.0.0.0/16 → local
```
can only route inside the VPC.

---

## 7. How to write answers in exam style

Use this sentence pattern:

```text
Connection [works/does not work] because the source subnet route table sends traffic to [local/IGW/NAT], and the destination security group [allows/does not allow] [protocol/port] from [source]. Since SGs are stateful, return traffic is allowed. If NACLs are custom, both inbound and outbound rules must also allow it.
```

Example:

```text
Internet → Web EC2 directly does not work. The web EC2 is in the app subnet, whose default route goes to the NAT Gateway, not the Internet Gateway, and the Web SG only allows TCP 80 from the ALB SG. The intended path is Internet → ALB → Web EC2.
```

Example:

```text
Web EC2 → RDS works if RDS SG allows TCP 3306 from Web SG. Routing is local because both subnets are inside 10.0.0.0/16. The DB subnet does not need an internet route for this connection.
```

---

## 8. Final cheat checklist

Before answering any VPC connection question, tick these:

```text
[ ] Source identified
[ ] Destination identified
[ ] Protocol/port identified
[ ] Same VPC or internet?
[ ] Source subnet route table checked
[ ] Public IP/EIP needed?
[ ] IGW needed?
[ ] NAT needed?
[ ] Destination SG inbound checked
[ ] Source SG outbound checked
[ ] NACL mentioned? If yes, check both directions
[ ] App/service actually listens on that port
[ ] For ALB: listener, target group, health check checked
[ ] For RDS: SG 3306 + DB credentials checked
[ ] For AWS API access: IAM role/policy checked
```

---

## 9. Tiny memory version

```text
Route = path
SG = door lock
NACL = subnet gate
IGW = public two-way internet
NAT = private outbound internet only
Local = private VPC-to-VPC routing
ALB = public entry, forwards to private targets
RDS = private DB, allow app SG on 3306
Bastion = admin jump box, but target SG must still allow it
```
