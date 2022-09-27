# Filtering Data with Labels
## Overview
There's many ways to slice and dice data but the main concepts come down to...
1) Creating a filtered data view (i.e. a filtered alias) that limits to a subset of a particular index's data based on some attribute (e.g. data stream name, field value). For example, if we had a **metrics-*** data stream that collected data across all systems, we could create a **filtered alias** called "team1-metrics" that only showed documents that had the label "team1".

2) Role-based access - through the Role, we can define what sets of data this particular user has access to. Following the example above, we could say "team1_role" only has access to **filtered aliases** that start with "team1". What does this mean? When you're logged in with this role, you can't see any data outside of what has been explicitly granted...so this prevents a person from Team 1 seeing data from Team 2.

3) The **filtered alias** and **roles** alone aren't enough to cover the scenario where a role has access to the appropriate data but now wants to see it in different logical groupings (for example, by Project).  This is where **Spaces** becomes useful as you can configure settings such as in APM or the Inventory views to only show information for a particular project.

In this example scenario, we have data coming from 2 projects that we created filtered aliases for. 
* We can create separate roles for each project if we have team members that can only should be able to see within their scope. Or a role that allows access to all data, such as for an SRE/DevOps group that oversees everything.
* We can then create separate Spaces for each Project that can house different sets of dashboards, data views, and customized settings for various Observability screens. 

**Note:** For the APM UI, you're able to use a **Service Environment** pull-down to switch between different Services that are being monitored with APM. Use the "service.environment" variable to make use of this, for example with .NET this gets set with ELASTIC_APM_ENVIRONMENT.  Reference: [Control Access to APM](https://www.elastic.co/guide/en/kibana/current/apm-spaces.html)

![image](https://user-images.githubusercontent.com/100947826/192630068-a1b40727-a0c4-4fab-aefb-21c7163b98fb.png)

## Scenario 1
I have different combinations of servers that support a particular application or group of applications.  I want to be able to see information for these servers in one area (without being cluttered by data from other servers and services being monitored).  

### Goal
* Add tag/label so that data coming from one IIS host can be separated from another IIS host on the Observability screens

### Setup
* 2 IIS servers
  * host.name 1: izmaxx-iis-lab3
  * host.name 2: izmaxx-iis-framework

### Adding Labels to Elastic Agent Outputs
<img width="699" alt="image" src="https://user-images.githubusercontent.com/100947826/192636481-b43b6ceb-993e-4a8d-8bbf-ccb52eb255bf.png">
<img width="881" alt="image" src="https://user-images.githubusercontent.com/100947826/192636585-db51d1e2-ad8a-49a6-bd47-17c8cf809055.png">




## Scenario 2
Multiple hosts with same application (load-balancer)...want to group these together on one Space

# Appendix
## Ingest Pipelines
This is a way to perform data processing before data is ingested into Elasticsearch from the Elastic Agent. It's another way to add fields that we can filter on for different purposes. For example, if we had a lookup that mapped **host.name** to a particular project, we could let the ingest pipeline handle this processing and add an appropriate label to the data that gets indexed. 

This would for example shift the maintenance on assigning tags/labels from configuration files on each host over to managing an index in Elasticsearch. Worth the effort? It depends. But another tool in the toolbelt.

References:
* https://www.elastic.co/guide/en/elasticsearch/reference/master/ingest.html#conditionally-run-processor
* https://www.elastic.co/guide/en/fleet/master/data-streams-pipeline-tutorial.html
* https://www.elastic.co/guide/en/fleet/master/data-streams.html#data-streams-pipelines
