# Bastion Host Setup and VPN Configuration

![AWS Architecture](https://i.imgur.com/kd2isyf.jpg)

## Overview

This guide details how to set up a Bastion Host and configure a VPN for secure access to instances in private subnets within an AWS VPC. A Bastion Host acts as a secure gateway between the internet and your private network, providing an additional layer of security.

## Table of Contents

- [What is a Bastion Host?](#what-is-a-bastion-host)
- [How a Bastion Host Works](#how-a-bastion-host-works)
- [Setting Up a Bastion Host](#setting-up-a-bastion-host)
- [VPN Setup Using Bastion Host](#vpn-setup-using-bastion-host)
  - [Steps to Set Up VPN Using a Bastion Host](#steps-to-set-up-vpn-using-a-bastion-host)
  - [Secure Access Flow](#secure-access-flow)
- [Resources](#resources)

## What is a Bastion Host?

A Bastion Host, also known as a jump server, is a special-purpose computer on a network specifically designed and configured to withstand attacks. It acts as a gateway between a trusted and an untrusted network, providing secure access to servers within a private subnet.

## How a Bastion Host Works

- **Isolation**: The Bastion Host is located in a public subnet and is the only instance exposed to the internet.
- **Access Control**: It uses strict security group rules to control access, typically allowing SSH connections only from specific IP addresses.
- **Jump Server**: Users first connect to the Bastion Host and then use it as a jump server to access instances in private subnets, ensuring those instances remain hidden from the public internet.
- **Audit and Monitoring**: All access through the Bastion Host can be logged and monitored for enhanced security.

## Setting Up a Bastion Host

1. **Launch an EC2 Instance**: Start an EC2 instance in a public subnet to act as the Bastion Host.
2. **Security Groups**: Create a security group that allows SSH access (port 22) only from your IP address and attach it to the Bastion Host.
3. **IAM Role**: Attach an IAM role with the `AmazonSSMManagedInstanceCore` policy to allow session manager access.
4. **Enable Session Manager**: Ensure that the instance has the SSM agent installed and configured to use AWS Systems Manager for secure access without needing an SSH key.

## VPN Setup Using Bastion Host

A VPN (Virtual Private Network) allows secure connections from remote networks to your AWS VPC. Using a Bastion Host with a VPN setup adds an extra layer of security.

### Steps to Set Up VPN Using a Bastion Host

1. **Create a VPN Gateway**:
   - Go to the VPC Dashboard.
   - Select “VPN Gateways” and create a new VPN gateway.
   - Attach the VPN gateway to your VPC.

2. **Create a Customer Gateway**:
   - This represents the remote device or network.
   - Specify the IP address of your customer gateway device and create the customer gateway.

3. **Create a VPN Connection**:
   - Specify the VPN gateway and customer gateway.
   - Choose the routing options (static or dynamic).

4. **Download Configuration**:
   - Download the VPN configuration file specific to your customer gateway device.

5. **Update Route Tables**:
   - Add routes to your private subnet route tables to send traffic destined for your remote network through the VPN connection.

6. **Configure the Bastion Host**:
   - Ensure the Bastion Host can route traffic from the VPN.
   - Update the Bastion Host’s security group to allow traffic from the VPN.

7. **VPN Client Configuration**:
   - Configure your VPN client or device using the downloaded VPN configuration file to connect to the AWS VPN gateway.

8. **Testing**:
   - Connect your VPN client to the VPN.
   - SSH into the Bastion Host and verify access to the private instances.

### Secure Access Flow

1. **VPN Connection**: Establish a VPN connection from the remote network to the AWS VPC.
2. **SSH to Bastion Host**: SSH into the Bastion Host using its public IP.
3. **Access Private Instances**: From the Bastion Host, SSH into the instances in the private subnets.

## Resources

For more details on setting up a Bastion Host and VPN on AWS, refer to the following resources:
- [AWS Bastion Host Guide](https://docs.aws.amazon.com/quickstart/latest/linux-bastion/architecture.html "AWS Bastion Host Architecture")
- [AWS VPN Documentation](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)

This setup ensures secure access to private instances, leveraging both a VPN and a Bastion Host to provide multiple layers of security.

### Authored by [Hitesh Vishwas Pogade](https://github.com/GetPlaced60)
