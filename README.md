# Lab M7.08 - VPC Endpoints and NAT Gateway Optimization

## Overview
In this lab, I designed and optimized a VPC architecture to reduce unnecessary NAT Gateway costs. The goal was to identify traffic patterns, eliminate avoidable data processing charges, and validate the impact using real CloudWatch metrics.

---

## Architecture

- VPC with CIDR `10.0.0.0/16`
- Public subnet with Internet Gateway access
- Private subnet with outbound access through a NAT Gateway
- EC2 instance deployed in the private subnet to generate traffic
- Gateway VPC endpoints for S3 and DynamoDB

---

## What I Did

- Created a VPC with public and private subnets
- Configured an Internet Gateway and a NAT Gateway
- Set up route tables for both subnets
- Launched an EC2 instance in the private subnet
- Generated baseline S3 and DynamoDB traffic through the NAT Gateway
- Collected CloudWatch metrics to measure NAT Gateway usage
- Created Gateway VPC endpoints for S3 and DynamoDB
- Verified that route tables were updated with prefix list routes
- Re-ran traffic tests after deploying endpoints
- Compared CloudWatch metrics before and after optimization

---

## Traffic Analysis

### Before VPC Endpoints
- All S3 and DynamoDB traffic flowed through the NAT Gateway
- Higher `BytesOutToDestination` and `BytesOutToSource` metrics observed
- Data processing costs applied to all traffic

### After VPC Endpoints
- S3 and DynamoDB traffic bypassed the NAT Gateway completely
- Significant drop in NAT Gateway traffic metrics
- Only minimal background traffic remained

---

## Key Findings

- Gateway endpoints for S3 and DynamoDB are free and highly effective
- NAT Gateway data processing ($0.045/GB) can become a major hidden cost
- CloudWatch metrics clearly show the impact of optimization
- Traffic was successfully redirected to the AWS internal network
- This optimization can be applied immediately in production environments

---

## Cost Impact

Although the lab uses small data volumes, the pattern scales significantly:

- Before: All traffic charged through NAT Gateway
- After: S3 and DynamoDB traffic cost reduced to $0
- Remaining cost: only NAT Gateway hourly charge

This approach can reduce monthly costs substantially in real workloads.

---

## Screenshots

- vpc-setup.png
- private-instance.png
- baseline-traffic.png
- nat-metrics-before.png
- s3-endpoint-route.png
- vpc-endpoints-list.png
- post-endpoint-traffic.png
- nat-metrics-after.png

---

## Conclusion

This lab demonstrates how a simple architectural improvement can significantly reduce AWS costs. By introducing Gateway VPC endpoints, it is possible to eliminate unnecessary NAT Gateway traffic, improve efficiency, and maintain secure private connectivity within AWS.

This is a high-impact optimization that should be standard practice in any production VPC.