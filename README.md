# AWS-Disaster-Recovery

**Project Overview**
This project demonstrates how to implement Backup and Disaster Recovery on AWS using EC2 and EBS Snapshots.
The goal was to safely back up an EC2 instance, simulate data loss, and successfully restore the system using snapshots.

**Technologies Used**
> Amazon EC2

> Amazon EBS

> EBS Snapshots

> Linux (Amazon Linux 2023)

> AWS Management Console

> AWS EC2 Instance Connect

**Project Objectives**
- Create a reliable backup of EC2 data using EBS snapshots
- Restore data from snapshots in case of failure
- Attach restored volumes to a recovery instance
- Verify data recovery using Linux commands.

**Architecture**
  EC2 Instance
   |
   |--> EBS Volume
           |
           |--> Snapshot (Backup)
                     |
                     |--> New Volume (Recovery)
                               |
                               |--> Attached to EC2


**Step-by-Step Implementation**

1️⃣ EC2 Instance Setup

Launched an EC2 instance in ap-south-1 (Mumbai) region
Installed Apache server and stored sample data

2️⃣ Create EBS Snapshot (Backup)

Created a snapshot of the attached EBS root volume
Snapshot stored safely in AWS for recovery
📸 Proof:<img width="1364" height="542" alt="Screenshot 2026-01-22 111451" src="https://github.com/user-attachments/assets/51e1fc9c-0d82-46e2-9c68-9ff2d675a57e" />

3️⃣ Restore Volume from Snapshot

Created a new EBS volume from the snapshot
Verified volume size and type (gp3)
📸 Proof:<img width="1358" height="533" alt="Screenshot 2026-01-22 115018" src="https://github.com/user-attachments/assets/17ced456-cf8c-4a5a-9d73-dc788a7b2026" />

4️⃣ Attach Volume to Recovery Instance

Attached restored volume to EC2 as a secondary disk
AWS NVMe device mapping observed

5️⃣ Mount Restored Volume (Recovery)

lsblk
sudo mkdir /recovery
sudo mount /dev/nvme1n1p1 /recovery
ls /recovery
📸 Proof:<img width="1360" height="543" alt="Screenshot 2026-01-22 111033" src="https://github.com/user-attachments/assets/23d4baf6-230a-4879-afbe-b7f2a6feeb06" />


**Challenge Faced & Solution**

Issue:
Mount failed using legacy device name /dev/xvdb1

Root Cause:
AWS Nitro instances expose volumes as NVMe devices

Solution:
Used lsblk to identify correct device (/dev/nvme1n1p1) and mounted successfully

