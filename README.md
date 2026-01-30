# AWS Migration & Cost Optimization Demo

This demo showcases an example cloud migration from a legacy EC2/EBS-based deployment to a modern **ECS Fargate microservices** architecture with cost optimization, autoscaling, and infrastructure-as-code (Terraform).

The goal of this repository is to demonstrate real-world AWS architecture thinking, system design, and operational optimization.

---

## 🏗️ 1. Legacy Architecture (BEFORE)

+-------------------------------+
|        EC2 Instance          |
|-------------------------------|
|  Node.js Monolithic Server    |
|  Static Assets + API          |
|  Background Jobs              |
+-------------------------------+
            |
            v
+-------------------------------+
|         EBS Volume           |
+-------------------------------+
            |
            v
    High Monthly Costs
    Manual Scaling
    Single Point of Failure

---

## 🚀 2. Modern Architecture (AFTER)

                        +----------------------------+
                        |       CloudFront CDN       |
                        +-------------+--------------+
                                      |
                                      v
+----------------+       +----------------------------+
|  S3 Static     | <---> |  API Gateway / ALB         |
|  Hosting       |       +-------------+--------------+
+----------------+                     |
                                      v
                              +--------------------+
                              |   ECS Fargate      |
                              |  Node.js Services  |
                              +---------+----------+
                                        |
                                        v
                                +---------------+
                                |   RDS/Aurora  |
                                +---------------+

---

## 💰 3. Cost Reduction Summary

Component | Before (EC2/EBS) | After (ECS Fargate)
--------- | ----------------- | --------------------
Compute   | High fixed cost   | Pay-per-use, scales
Storage   | EBS charges       | S3 cheaper
Deploys   | Manual            | Automated CI/CD
Scaling   | Manual            | Autoscaling

---

## 🛠️ 4. Terraform Sample

resource "aws_ecs_cluster" "demo" {
  name = "demo-cluster"
}

resource "aws_ecs_task_definition" "node_service" {
  family                   = "node-service"
  requires_compatibilities = ["FARGATE"]
  cpu                      = "256"
  memory                   = "512"
  network_mode             = "awsvpc"

  container_definitions = jsonencode([
    {
      name  = "app",
      image = "public.ecr.aws/node:latest",
      portMappings = [
        {
          containerPort = 3000
        }
      ]
    }
  ])
}

resource "aws_ecs_service" "node_service" {
  name            = "node-service"
  cluster         = aws_ecs_cluster.demo.id
  task_definition = aws_ecs_task_definition.node_service.arn
  desired_count   = 1
  launch_type     = "FARGATE"

  network_configuration {
    subnets         = ["subnet-123456"]
    assign_public_ip = true
  }
}

---

## 🎯 Purpose

This repository showcases experience in:

- Cloud migrations
- Cost optimization
- ECS/Fargate deployments
- Infrastructure as Code (Terraform)
- Scalable architecture design

---

## 📬 Contact

**Email:** faraz.munavarhussain@gmail.com  
**LinkedIn:** https://www.linkedin.com/in/faraz-munavar-hussain-bb164137
