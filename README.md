# cloudlab
Virtual Homelab Setup on AWS Public Cloud

## Architecture
The basic layout of the lab is as follows:
![Network Diagram](https://raw.githubusercontent.com/vishnuvardhan-kumar/cloudlab/master/homelab.JPG)

## Quick Facts
- 12 AWS EC2 Instances in total
- Full-blown [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) setup
- Securely accessible from anywhere on the internet
- Funded by AWS Educate credits that I earned in school/college.

## WireGuard
- Only EC2 instance that is exposed to the internet (Bastion Host)
- Hardened firewall and OS configuration
- Provides VPN access to other instances and physical homelab
- Authentication provided through AD

## SSO / Authentication
- Consists of 2 EC2 instances in high-availability mode running Windows Server 2016
- Both instances serve as Domain Controllers
- Installed services: AD-DS, AD-CS, DNS

## Development Subnet
- Consists of 4 EC2 instances, each running a single tool/service (AD authentication on all)
- GitLab: self-hosted instance (source-control, code-linting/analysis)
- Jenkins: CI/CD workflows and pipelines
- Ansible: Runs Ansible processes and AWX
- Docker: Bunch of containers (deployment target for Jenkins/Ansible)

## Pentesting Subnet
- Consists of 4 EC2 instances, 
- Master instance: Runs Kali Linux 2020.1 with 2x AWS Elastic Graphics (for GPU accelerated processes/parallelization)
- Test instance 1: Runs Windows Server 2012 (+ AD, used to mimic domain environment)
- Test instance 2: Runs RHEL 7 (hardened target node)
- Test instance 3: Runs FreeBSD 12 (modified to mimic macOS High Sierra internals)

## Monitoring
- EC2 instance running Prometheus + Grafana stack
- Agents running on all EC2 instances (node_exporter for Linux / wmi_exporter for Windows)
- Grafana Dashboard shows live status of all services (screenshot below)

![Grafana](https://raw.githubusercontent.com/vishnuvardhan-kumar/cloudlab/master/grafana.JPG)

