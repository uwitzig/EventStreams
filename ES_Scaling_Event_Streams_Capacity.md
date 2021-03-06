---

copyright:
  years: 2020
lastupdated: "2020-09-25"

keywords: IBM Event Streams, scaling capacity

subcollection: EventStreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# Scaling Enterprise plan capacity
{: #ES_scaling_capacity}


## Event Streams Capacity
{: #ES_capacity}

The {{site.data.keyword.messagehub}} Enterprise plan allows you to specify throughput and storage capacity when a new instance of the service is created. 
If, after using the service instance, you find that the current capacity configuration of your service instance is not meeting the demands of your solution, 
throughput and/or storage capacity can be scaled-up to meet demands.

Each base or additional capacity unit includes the following:
* 150 MB/s of throughput capacity.
* 2 TB of storage capacity available for your data retention.

For example, selecting a base capacity unit, one additional capacity unit, and 4 TB of additional storage provides you with the following:
* 300 MB/s of throughput capacity.
* 8 TB of storage capacity for data retention.


## Throughput Capacity
{: #ES_thruput_capacity}

Throughput capacity is the recommended peak MB/s maximum for producing and consuming messages. 

Each capacity unit provides 150 MB/s of throughput capacity. This is comprised of 75 MB/s data ingress and 75 MB/s data egress capacity.

To scale-up throughput capacity, you can add additional capacity units. Each additional capacity unit adds 150 MB/s of throughput to your service instance, to a total of 450 MB/s.

The recommended throughput maximum is based on a typical workload and takes into account the possible impact of operational actions or failure modes, like the loss of an availability zone. 
If the average throughput exceeds the recommended figure, a loss in performance might be experienced during these conditions. It is recommended to plan your maximum throughput capacity 
as two-thirds of the peak maximum. For example, two-thirds of the 150 MB/s peak maximum is 100 MB/s. For additional information on capacity recommendations and limitations, 
refer to [limits and quotas](/docs/EventStreams?topic=EventStreams-kafka_quotas#limits_enterprise).

Throughput scaling is independent of storage, however for each tier there is a defined minimum storage amount required. 

## Storage Capacity
{: #ES_storage_capacity}

Storage capacity is the amount of storage allocated in the service instance for retention of message data. 

Storage capacity can be scaled-up, independent of throughput capacity, when data retention is important for your architecture.

{{site.data.keyword.messagehub}} stores three replicas of your data to ensure the highest level of resilience across three availability zones. 
When you select 2 TB of storage with {{site.data.keyword.messagehub}}, it is equivalent to deploying 6 TB of storage if you are running your own Apache Kafka cluster 
with the same replication policy enabled.

## Scaling Combinations
{: #ES_scaling_combinations}

The table below lists valid throughput/storage capacity unit combinations.

|**Throughput capacity**|**Available storage capacity**|
|-------------------|--------------------------|
|150 MB per second (75 MB/s producing, 75 MB/s consuming)|2 TB, 4 TB, 6 TB, 8 TB, 10 TB, 12 TB|
|300 MB per second (150 MB/s producing, 150 MB/s consuming)|4 TB, 8 TB, 12 TB|
|450 MB per second (225 MB/s producing, 225 MB/s consuming)|6 TB, 12 TB|

For additional information on capacity limitations, refer to [limits and quotas](/docs/EventStreams?topic=EventStreams-kafka_quotas#limits_enterprise).

Throughput capacity cannot be scaled down. To move to a lower throughput capacity would require creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.
{:important}

Storage capacity cannot be scaled down. To move to a lower storage capacity would require creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.
{:important}

## How to scale capacity
{: #ES_how_to_scale_capacity}

The steps below show you how to scale-up throughput and/or storage capacity for an {{site.data.keyword.messagehub}} Enterprise plan service instance. 
If you do not have an Enterprise instance, the steps below will help you to create one.

At this time, scaling-up an {{site.data.keyword.messagehub}} service instance capacity requires the use of the {{site.data.keyword.Bluemix_notm}} CLI.

To install this tool, see [install devtools](/docs/cli?topic=cli-install-devtools-manually#install-devtools-manually).

The {{site.data.keyword.Bluemix_notm}} CLI command will use the **service-instance-update** command to update your {{site.data.keyword.messagehub}} service instance resource. 
The user ID in the account used to issue the **service-instance-** command must be assigned the same access policies that are needed when creating resources. 
See [creating resources](/docs/account?topic=account-manage_resource#creating-resources) for access requirements.

### During the scale-up process

The time required to scale-up the {{site.data.keyword.messagehub}} service instance is variable. Both, throughput and storage, require provisioning extra infrastructure.

During this time, Kafka topic/partition add/update/delete operations will be suspended. This ensures that the integrity of data is maintained during 
storage volume infrastructure scale-up operations. This suspension of topic/partition operations will only occur during a brief portion of the scale-up process, not the entire process.

Valid combinations/values for the "throughput" and "storage_size" are the following:

|**Throughput capacity (peak maximum)**|**"throughput" value to specify**|**Storage capacity**|**"storage_size" value to specify**|
|----------------------------------------|-----------------------------|----------------------|------------------------------|
|1  (150 MB/s )|150|2 TB|2048|
| | |4 TB|4096|
| | |6 TB|6144|
| | |8 TB|8192|
| | |10 TB|10240|
| | |12 TB|12288|
|2  (300 MB/s )|300|4 TB|4096|
| | |8 TB|8192|
| | |12 TB|12288|
|3  (450 MB/s )|450|6 TB|6144|
| | |12 TB|12288|



### Example

Deploy a service instance configured with a base capacity unit:
* 150 MB/s of throughput capacity.
* 2 TB of storage capacity for data retention.

Scale this service instance to a configuration of a base capacity unit, one additional capacity unit, and 4 TB of additional storage providing:
* 300 MB/s of throughput capacity.
* 8 TB of storage capacity for data retention.

1. If you don't already have one, create an {{site.data.keyword.messagehub}} service instance.
  
    a. Log in to the **{{site.data.keyword.Bluemix_notm}} console**.
    
    b. Click the **{{site.data.keyword.messagehub}} service** in the Catalog.
    
    c. Select the **Enterprise plan** on the service instance page.
    
    d. Review capacity selections of 150 MB/s throughput and 2 TB storage.
        
    g. Enter a name for your service instance. You can use the default value.
    
    h. Click Create. (See [choosing your plan](/docs/EventStreams?topic=EventStreams-plan_choose#what-s-supported-by-the-lite-standard-enterprise-and-classic-plans) 
    for information on the amount of time needed to create the service instance).

2. Log in to the **{{site.data.keyword.Bluemix_notm}} CLI**.
 
      <code>ibmcloud login</code>

3. Get the resource name of your {{site.data.keyword.messagehub}} service instance.
  
      <code>ibmcloud resource service-instances</code>
     
      (You can find the name of your instance in the Name column.)

4. View the current capacity configuration using the {{site.data.keyword.messagehub}} CLI.
    
    To install and use the CLI plugin, refer to [cli reference](/docs/EventStreams?topic=EventStreams-cli_reference).
    
    Use the following command to display the current capacity configuration:
  
      <code>ibmcloud es init  --instance-name  "Event Streams resource instance name"</code>

    Output will be similar to the following, which shows this service instance is configured with 150 MB/s of throughput capacity and 2 TB of storage capacity:

        API Endpoint:         https://service-instance-adsf1234asdf1234asdf1234-0000.eu-south.containers.appdomain.cloud
        Service endpoints:    public
        Storage size:         2048 GB
        Throughput:           150 MB/s


5. Scale-up the service instance from **150 MB/s throughput capacity and 2 TB storage capacity** to **300 MB/s throughput capacity and 8 TB storage capacity**. 
    
    a. Execute the following from the cli:
    
      <code> ibmcloud resource service-instance-update "Event Streams resource instance name" -p '{"throughput":"300","storage_size":"8192"}' </code>

    b. If there is an issue running the ibmcloud resource service-instance-update command and requires contacting IBM Support for assistance, 
    run the following command and include the output when contacting support:

      <code> ibmcloud resource service-instance "Event Streams resource instance name" --output=json </code>

6. Monitor the update of the service instance.

    The scale-up process could take several minutes to complete depending on what new resources need to be allocated to the service instance.
    
    You can get the current service instance information using the following command:
    
      <code> ibmcloud resource service-instance "Event Streams resource instance name" --output=json </code>
        
    Review the Last Operation section of the output. The information will be continuously updated as the update proceeds. When the scale-up process has completed, 
    the last operation information will indicate update succeeded or sync succeeded.

    Rerun the command until success is indicated.

7. Verify the scaled-up capacity configuration using the {{site.data.keyword.messagehub}} CLI.
  
    Display the capacity configuration with the following command:
    
      <code> ibmcloud es init  --instance-name  "Event Streams resource instance name" </code>
        
    Output will be similar to the following, which shows that this service instance is configured with 300 MB/s of throughput capacity and 8TB of storage capacity:


       API Endpoint:         https://service-instance-adsf1234asdf1234asdf1234-0000.eu-south.containers.appdomain.cloud
       Service endpoints:    public
       Storage size:         8192 GB
       Throughput:           300 MB/s

