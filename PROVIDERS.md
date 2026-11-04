# Getting a server

Any Ubuntu server with a public IP works. Below are step-by-step instructions per provider.

## AWS (EC2)

### 1. Launch the instance

AWS Console → pick a region near you (top right) → **EC2 → Launch instance**:

- **Name**: `cicd-demo`
- **AMI**: Ubuntu Server 24.04 LTS
- **Instance type**: `t3.micro` or `t2.micro` (free-tier eligible). Avoid `t4g.*`: they are ARM, and the image built by GitHub Actions is x86.
- **Key pair**: *Create new key pair* → ED25519, `.pem` → keep the downloaded `cicd-demo.pem`.
- **Network settings**: tick
  - *Allow SSH traffic from* **Anywhere** (not "My IP": GitHub Actions connects from changing IPs),
  - *Allow HTTP traffic from the internet*.
- Leave the rest as is → **Launch instance**.

Copy the **Public IPv4 address** from the instance page.

### 2. Install Docker

```bash
chmod 400 cicd-demo.pem
ssh -i cicd-demo.pem ubuntu@<public-ip>
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker ubuntu
exit
```

Reconnect and check that `docker ps` works without `sudo`.

### 3. Continue with the README

Go on with step 2 of the README (SSH key for CI), using:

- `SSH_HOST` = the public IP
- `SSH_USER` = `ubuntu`

### Good to know

- **The public IP changes if you stop and start the instance.** For a fixed one, use EC2 → *Elastic IPs* → *Allocate* → *Associate* with your instance, and use that IP as `SSH_HOST`.
- **Set a budget alert first**: Billing → *Budgets* → *Create budget* → *Zero spend budget*.
- **When you're done**: *Terminate* the instance (stopping it is not enough) and release the Elastic IP if you made one.
