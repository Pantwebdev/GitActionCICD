# AWS EC2 deployment

The GitHub Actions workflow builds and publishes the Spring Boot image to GitHub Container Registry, then deploys it to an EC2 instance over SSH.

## EC2 setup

Use an Ubuntu EC2 instance and install Docker:

```bash
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker "$USER"
```

Log out and back in after adding the user to the Docker group. In the EC2 security group, allow inbound TCP traffic on ports `22` and `80`. The application is available at `http://<EC2_PUBLIC_IP>/`.

## GitHub repository secrets

Add these secrets under **Settings > Secrets and variables > Actions**:

| Secret | Value |
| --- | --- |
| `EC2_HOST` | EC2 public IP address or DNS name |
| `EC2_USERNAME` | Usually `ubuntu` for Ubuntu AMIs |
| `EC2_SSH_KEY` | The complete private SSH key for the instance |
| `GHCR_USERNAME` | GitHub username that owns the container package |
| `GHCR_TOKEN` | GitHub PAT with `read:packages` permission |

Push to `master` or `main` to build, publish, and deploy the latest image.