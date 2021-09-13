# Resource Management in GCP

## Summary



For a quick overview of what services are available in which zone, look at [GCP Cloud Locations](https://cloud.google.com/about/locations)

## Services: zonal, regional, global

Global services: 

Some examples of global services are:
- IAM (permissions, etc. are not tied to any zone: they are applied globally)
- Cloud interconnect (you can connect to a peering point neart you but the effects are not locked to any zone)

## Projects, Folders, and Organizations

In GCP every resource must be part of a project.
Projects are the fundamental way to manage resources in Google Cloud.
All billing goes to a project.
Every project has its VPC (Virtual Private Cloud).




