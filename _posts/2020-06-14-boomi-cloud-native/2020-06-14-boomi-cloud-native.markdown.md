---
layout: post
title:  "Boomi runtime with Cloud Native Platforms"
permalink: /boomi-cloud-native/
---
Introduction
============

Installation of Boomi runtime, Atom and Molecule, can be done very quickly: once you download the binaries and fulfill the prerequisites in term of network and storage, you just need few clicks to have a runtime up and running and ready to run your integration processes.

Scripted installation of Boomi Atom and Molecule
================================================

Despite this speed, some organizations want to go even faster on the deployment side and are using provisioning tools like Ansible, Puppet and Chef.  
These organizations can leverage the silent installation option of the runtime installers.  

For instance, for a Multi-nodes Molecule, we have 2/3 steps

1.  Installation of the first Node by running the installer with the specific parameters, more details here: [Unattended installation of Boomi runtime](https://help.boomi.com/bundle/integration/page/c-atm-Unattended_installation_of_Atom_Molecule_or_Cloud_8e4eb1de-e2f9-40d1-888b-a9c25e395997.html)
2.  For a Molecule, ddding of a node. This step is very simple: as long as the new server/VM has access to the Shared Folder, you just need to run the “./atom start” command from the Molecule bin directory to have the new node added to the Molecule. More details here: [Adding Molecule nodes](https://help.boomi.com/bundle/integration/page/t-atm-Installing_additional_Molecule_nodes_on_Linux_3fec362e-e44e-4859-baa6-1882b6fb420a.html)
3.  (Optional) If the Molecule is used to expose Web-Services/REST APIs, the Reverse Proxy in front of the Molecule need to be configured to use the new computing power of the additional node

Containers and Scheduling/Orchestration Platforms
=================================================

Boomi installers are coming with three mains flavors: Windows, Linux and Docker. The first two flavors where used in the previous use-case. When it comes to containers, we will use the third flavor.  
This Docker version or the Atom or the Molecule can be used with multiple Docker-compatible Container Orchestration platform.  
Please note that Boomi is also offering specific versions of the runtime for specific Container Orchestration platform, like Pivotal PKS or VMWare Tanzu.

Container Orchestration Platform summary
----------------------------------------

The following table is trying to summarize some possibilities:

| Container Orchestration Platform                                                                                                                                              | Setup Method | References |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| -- | -- |
| Docker-Swarm-like platform: <br> *   AWS EC2 Instances with Docker Swarm <br> *   Azure VMs <br> *   Docker Swarm on VMs <br> *   Docker Swarm on Bare-metal | *   Using Docker command line if available <br> *   Using custom Task/Pod definition | [Boomi Help](https://help.boomi.com/bundle/integration/page/int-Docker_Molecule_installation_checklist_Linux_fa5d6d80-5789-4cc3-925d-d927286ad558.html) |
| AWS ECS (EC2 or Fargate)                                                                                                                                                      | *   Using AWS ECS using JSON Task Definition  | [https://github.com/anthonyrabiaza/BoomiAWSECS](https://github.com/anthonyrabiaza/BoomiAWSECS) |
| Kubernetes (K8s) <br> *   AWS EKS <br> *   Azure AKS <br> *   K8s on VMs <br> *   K8s on Bare-metal                                                                           | Using K8s yaml files with Boomi Docker images | [https://github.com/anthonyrabiaza/BoomiKubernetes](https://github.com/anthonyrabiaza/BoomiKubernetes) |
| Pivotal Container Service / PKS                                                                                                                                               | Using Boomi Data Services PKS from Pivotal | [https://github.com/anthonyrabiaza/BoomiPivotalContainerService](https://github.com/anthonyrabiaza/BoomiPivotalContainerService)  <br> [https://network.pivotal.io/products/boomi-data-services-pks/](https://network.pivotal.io/products/boomi-data-services-pks/) |
| Pivotal PCF / VMWare Tanzu                                                                                                                                                    | Using Boomi Data Services from Pivotal | [https://network.pivotal.io/products/boomi-data-services/](https://network.pivotal.io/products/boomi-data-services/) |
| OpenShift                                                                                                                                                                     | Using K8s yaml files with custom Boomi Docker images | [https://github.com/anthonyrabiaza/BoomiOpenShift](https://github.com/anthonyrabiaza/BoomiOpenShift) |


Examples of Container Orchestration Platform deployments
--------------------------------------------------------

### Sample High-level diagram for a Molecule Deployment on Kubernetes

 ![Boomi with K8s](/assets/boomi-cloud-native/kubernetes.png)

### Sample High-level diagram for a Molecule Deployment on Pivotal PKS

![Boomi with PKS](/assets/boomi-cloud-native/pivotal-pks.png)

### Sample High-level diagram for a Molecule Deployment on OpenShift

![Boomi with OpenShift](/assets/boomi-cloud-native/openshift.png)