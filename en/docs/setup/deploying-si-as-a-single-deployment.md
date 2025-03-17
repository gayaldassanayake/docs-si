# Single Node Deployment

You can deploy the WSO2 Streaming Integrator as a single node deployment to achieve most of the use cases that commonly arise
in the streaming integration world. The other deployment options, namely [Minimum High Availability (HA) Deployment](deploying-si-as-minimum-ha-cluster.md) and
[Scalable High Available(HA) Deployment](deploying-si-as-a-single-deployment.md) are mainly introduced to handle high availability, scalability, and resiliency.
However, single node deployment too allows you to achieve resilient deployment as explained in the [Resilient Deployment](#resilient-deployment.md) subsection.

!!! tip "System Requirements"
    For system requirements for this deployment, see [Installing the Streaming Integrator in a Virtual Machine](installing-si-in-vm.md).

## Resilient deployment

Resiliency guarantees the ability to withstand or recover from any system failure and carry out the process without 
loosing any data. Streaming integrator has the capability to achieve the above via a broker that can replay data from a
certain checkpoint. Kafka is one such broker that you can configure with WSO2 Streaming Integrator to achieve this. The only additional configuration that you need to do in WSO2 Streaming Integrator is state persistence. For detailed
instructions, see [Configuring Database and File System State Persistence](../admin/configuring-Database-and-File-System-State-Persistence.md).

##![overview]({{base_path}}/images/singleNodeDeployment.jpg)

If the single Streaming Integrator node fails to receive incoming events and if you have configured state persistence, the single node is able to retrieve the latest snapshot from the database and request the broker to send the events that it was unable to process due to the failure


The only disadvantage of this approach is that you need to ensure that there is a reliable mechanism to restart the server once it fails.
