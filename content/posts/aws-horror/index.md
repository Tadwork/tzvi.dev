---
title: "Ignorance is not bliss: an AWS horror story"
date: 2023-11-12T09:03:20-08:00
# featuredImage: "posts/bugs/bugs.webp"
draft: true
tags: ["cloud computing", "aws", "ECS", "costs"]
description: "A story of how a team I worked with accidentally spent ~$20k+ in a month in cloud computing costs"
---

## The software stack

ECS managed workers created in isolated environments through terraform/terragrunt configuration files. 

## Costs soaring

first noticed an issue when costs had risen significantly over a month. It wasn't clear if this was related to new functionality and would get lower over the course of the following months as usage leveled out. 

Investigation by a team member revealed that costs were related to egress costs. further investigation narrowed it down to a particular ip address and subnet which turned out to be a non production environment. It was puzzling that a non-production workload could be racking up such high egress costs.

## identifying the source

After looking at the services in use by this environment the team noticed that there were some ECS services that were not starting correctly and were in a restart loop. Since the ECS cluster did not have a direct connection to AWS to pull the docker image from ECR it was doind so over a natted connection - causing the XXXXX GB docker image to be pulled constantly for each of the 5 containers trying unsuccesfully to restart. This was causing an average XXXX of egress traffic over the course of a single day!


## THe fix

