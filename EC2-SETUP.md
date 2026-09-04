# EC2 Instance Setup Guide

How to launch a Ubuntu t2.medium EC2 instance on AWS to run this Docker project.

---

## 1. Open the EC2 Console

Go to the [AWS EC2 Console](https://console.aws.amazon.com/ec2/) and click **Launch instance**.

> Make sure you're in the correct AWS Region (top-right of the console). Your instance will be created there.

---

## 2. Name Your Instance

Under **Name and tags**, give your instance a name (e.g. `docker-mastery-server`).

This is just a label — it becomes a tag with key `Name` on the instance.

---

## 3. Choose the AMI (Amazon Machine Image)

Under **Application and OS Images**, select:
- **Ubuntu** from the Quick Start options
- Choose **Ubuntu Server 22.04 LTS (HVM), SSD Volume Type**
- Architecture: **64-bit (x86)**

> An AMI is a pre-built OS image that your instance boots from. Ubuntu 22.04 LTS is a stable, long-term support release — a safe default for servers.

---

## 4. Choose Instance Type

Under **Instance type**, select **t2.medium**.

| Spec | Value |
|---|---|
| vCPUs | 2 |
| Memory | 4 GB |
| Network | Moderate |

> t2.medium gives enough CPU and RAM to comfortably run Docker containers locally. t2.micro (free tier) is too small for running multiple containers.

> t2 instances are "burstable" — they accumulate CPU credits when idle and spend them during bursts of activity.

---

## 5. Create a Key Pair

Under **Key pair (login)**, click **Create new key pair**:
- **Name**: `docker-mastery-key` (or any name you'll remember)
- **Type**: RSA
- **Format**: `.pem` (for SSH on Mac/Linux)

Click **Create key pair** — your browser will download the `.pem` file automatically.

> This is your only way to SSH into the instance. Keep the `.pem` file safe — AWS does not store it and you cannot download it again.

Move it somewhere safe and restrict its permissions:
```bash
mv ~/Downloads/docker-mastery-key.pem ~/.ssh/
chmod 400 ~/.ssh/docker-mastery-key.pem
```

---

## 6. Configure Network & Security Group

Under **Network settings**, click **Edit** and configure:

- **VPC**: leave as default
- **Subnet**: leave as default
- **Auto-assign public IP**: Enable

Create a new **Security Group** with these inbound rules:

| Type | Protocol | Port | Source | Why |
|---|---|---|---|---|
| SSH | TCP | 22 | My IP | SSH access from your machine only |
| Custom TCP | TCP | 8000 | 0.0.0.0/0 | Phase 1 app |
| Custom TCP | TCP | 8001 | 0.0.0.0/0 | Phase 2 app |
| Custom TCP | TCP | 8002 | 0.0.0.0/0 | Phase 3 app |

> A Security Group is a virtual firewall. By default all inbound traffic is blocked — you explicitly allow only what's needed. Restricting SSH to **My IP** means only your machine can log in.

---

## 7. Configure Storage

Under **Configure storage**, set:
- **Size**: 20 GB (default 8 GB is tight once Docker images are pulled)
- **Type**: gp3 (General Purpose SSD)

---

## 8. Launch the Instance

In the **Summary** panel, review your config and click **Launch instance**.

AWS will show a success screen with your instance ID. Click it to go to the instance details page.

Wait ~1–2 minutes for the **Instance State** to show **running** and the **Status checks** to pass.

---

## 9. SSH Into the Instance

On the instance details page, copy the **Public IPv4 address**.

Then SSH in from your terminal:
```bash
ssh -i ~/.ssh/docker-mastery-key.pem ubuntu@<your-public-ip>
```

> The default username for Ubuntu AMIs is `ubuntu` (not `ec2-user` or `root`).

If you get a permission warning, make sure the key file has correct permissions (`chmod 400`).

---

## 10. Install Docker on the Instance

Once you're SSH'd in, follow the steps in [DOCKER-INSTALLATION.md](DOCKER-INSTALLATION.md) to install Docker.

Then clone this repo and run any phase:
```bash
git clone https://github.com/vishakhasadhwani/docker-mastery-project
cd docker-mastery-project/phase1-beginner
docker build -t docker-mastery:phase1 .
docker run -d --name phase1-app -p 8000:8000 docker-mastery:phase1
```

Visit `http://<your-public-ip>:8000/docs` in your browser.

---

## Stop vs Terminate

| Action | What it does |
|---|---|
| **Stop** | Shuts down the instance, keeps the disk. You are not charged for compute time while stopped, but storage charges still apply. |
| **Terminate** | Permanently deletes the instance and its storage. Cannot be undone. |

> Always **Stop** the instance when you're done working to avoid unexpected charges. Only **Terminate** when you're sure you no longer need it.
