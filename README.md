# AWS-Architecture
This is the one architecture diagram most DevOps engineers keep in their head. If you truly understand this, 80% of AWS architectures become obvious.
Step-by-step flow
# 1. User enters domain

The request starts with DNS handled by Amazon Route 53.

Example:

example.com → ALB

Route53 only resolves the domain to the load balancer address.

# 2. Internet traffic enters AWS

Traffic reaches the VPC through the Internet Gateway attached to the Amazon VPC.

Inside the VPC we typically separate resources into:

Public Subnet
Private Subnet
# 3. Public subnet layer

Public subnet usually contains internet-facing components:

Application Load Balancer

NAT Gateway

Example route table:

0.0.0.0/0 → Internet Gateway
# 4. Load balancer distributes traffic

The load balancer sends traffic to backend servers running on Amazon EC2.

Example:

User request
↓
ALB
↓
EC2 instance 1
EC2 instance 2
EC2 instance 3

This improves availability and scalability.

# 5. Private subnet (application layer)

Application servers are usually placed in private subnets.

Reason:

Security

They should not be reachable from the internet directly.

Traffic flow:

ALB → EC2
# 6. Database layer

The database (often Amazon RDS) sits in private subnets as well.

Example access rule:

RDS inbound:
3306 from EC2 security group

This prevents direct internet access.

# 7. Private servers accessing internet

Private servers still need to:

download updates

install packages

call APIs

So they use:

EC2 → NAT Gateway → Internet

The NAT allows outbound traffic only.

Final architecture flow
User
↓
Route53
↓
Internet
↓
ALB (public subnet)
↓
EC2 Auto Scaling (private subnet)
↓
RDS database
Key design principle

Production systems usually follow three layers:

Edge layer → Load balancer
Application layer → EC2
Data layer → Database

Each layer is isolated and secured.

One important thing you should notice

Security is layered:

NACL
↓
Security Groups
↓
Application firewall

Multiple layers protect the infrastructure.
