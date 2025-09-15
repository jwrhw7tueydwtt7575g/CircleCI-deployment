# CircleCI Docker Deployment Project

This project demonstrates a complete CI/CD pipeline using CircleCI to build, test, and deploy a Python application as a Docker container to an Amazon EC2 instance.

## Features
- Automated dependency installation and testing with pytest
- Docker image build using an inline Dockerfile
- Docker image transfer and deployment to an EC2 instance
- Secure SSH key management for deployment

## Project Structure
```
application.py           # Main Python application
requirements.txt         # Python dependencies
setup.py                 # Python package setup
.circleci/config.yml     # CircleCI pipeline configuration
artifacts/               # Model and processed data artifacts
logs/                    # Log files
notebook/                # Jupyter notebooks
pipeline/                # Training pipeline scripts
src/                     # Source code (data processing, model training, etc.)
static/, templates/      # Web static files and templates
```

## CircleCI Pipeline Overview

### 1. Build Job
- Installs Python dependencies
- Runs unit tests with pytest

### 2. Docker Build Job
- Creates a Dockerfile on-the-fly
- Builds the Docker image for the application
- Saves the image as `myapp.tar` for deployment

### 3. Deploy Job
- Loads SSH keys for secure EC2 access
- Copies the Docker image to the EC2 instance
- Loads and runs the Docker container on EC2

## How to Use

### Prerequisites
- An Amazon EC2 instance (Amazon Linux recommended)
- Docker installed on the EC2 instance
- CircleCI project with SSH key added (private key in CircleCI, public key in EC2's `~/.ssh/authorized_keys`)
- EC2 security group allows SSH and app port (e.g., 80)

### Setup Steps
1. **Clone this repository**
2. **Configure your EC2 instance:**
   - Install Docker: `sudo yum install docker -y && sudo service docker start`
   - Add CircleCI public SSH key to `/home/ec2-user/.ssh/authorized_keys`
3. **Add your EC2 private key to CircleCI:**
   - Go to Project Settings → SSH Keys → Add SSH Key
   - Set hostname to your EC2 public IP
4. **Update `.circleci/config.yml` with your EC2 public IP and user (`ec2-user`)**
5. **Push your code to trigger the pipeline**

## Example CircleCI Workflow
```
workflows:
  version: 2
  build_test_deploy:
    jobs:
      - build
      - docker_build:
          requires:
            - build
      - deploy:
          requires:
            - docker_build
```

## Troubleshooting
- **Permission denied (publickey):** Ensure the CircleCI public key is in EC2's `authorized_keys`.
- **No tests ran:** Add test files named `test_*.py` with functions prefixed by `test_`.
- **YAML parse errors:** Check indentation and syntax in `.circleci/config.yml`.

## License
MIT

## Author
Your Name
