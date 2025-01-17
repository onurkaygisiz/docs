---
description: This page introduces important key features of aedifion.io
---

# Features

## Overview

This page offers a brief and high-level description of all important features of the aedifion.io platform roughly sorted by the different steps of a typical data processing pipeline: We start with with the different ways of ingressing data, continue with the functional aspects of the storage, processing, and management of data, then move on to the egress of data as well as the generation of higher level insights from the data and, finally, conclude with non-functional features such as security, privacy, and the legal framework.

## Data ingress

aedifion.io offers plant-, building-, and even district-wide data acquisition from diverse data sources. The platform speaks most industry bus communication standards and can automatically ingest data from your plant, building, or district using auto-discovery of all available [datapoints ](https://docs.aedifion.io/docs/glossary#datapoint)and [devices](https://docs.aedifion.io/docs/glossary#device) for easy set-up. Furthermore, the platform integrates various other IP-based data sources, e.g., ranging from your existing legacy data bases and servers over your Exchange server to third-party data sources such as weather forecasts. Finally, live data can be streamed directly into aedifion.io via MQTT while historic and batch data can be ingested using file upload in different formats such as CSV.

| Data source | Example |
| :--- | :--- |
| Automation networks | BACnet, Modbus, KNX, M-Bus, OPC UA/DA, ... |
| Databases | Different flavors of SQL and NoSQL, Influx, OpenTSDB, ... |
| Third party APIs | MS Exchange, weather data and forecasts, ... |
| User | MQTT stream, file upload, ... |

{% hint style="success" %}
aedifion.io works fully plug-and-play for BACnet systems. All it needs is the aedifion.io edge device that operating personnel simply connect to the local \(building\) network. Once plugged in, the edge device connects automatically to our servers and we configure, start, and continuously monitor the data ingress.
{% endhint %}

_Learn more? Explore our_ [_data import and export tutorial_](../developers/api-documentation/guides-and-tutorials/data-import.md)_._

### Meta data

For each integrated [device](https://docs.aedifion.io/docs/glossary#device) and [datapoint](https://docs.aedifion.io/docs/glossary#datapoint), aedifion.io is able to acquire and maintain comprehensive meta data either directly via the [automation network](../glossary.md#automation-network), e.g., reading BACnet properties or Modbus plant descriptions, or from local databases and servers such as OPC. All meta data is automatically structured in aedifion.io's data model and can be used from thereon to search and sort the data as well as to enrich it further using artificial intelligence methods such as clustering and classification.

### AI-generated meta data

Our platform augments the existing the Meta data obtained from the automation network with Artificial Intelligence \(AI\)-generated information. Each datapoint and its observations are analyzed and an AI generates annotations from a set of predefined classes. The newly generated descriptions can be searched and filtered to organize your building. Since this is a statistical decision process, each annotation also provides a probability on the AI's confidence.

### Weather and weather forecast data

aedifion.io automatically supplies local weather data and forecast data for each project, depending on its GPS coordinates.

_Learn more? Explore what_ [_weather data_](data/#weather-data) _is available on aedifion.io._

## Data storage and resolution

For high-volume time series data, aedifion.io uses specialized [time series](https://docs.aedifion.io/docs/glossary#time-series) databases that, among others, allow highly efficient queries and processing over time ranges of that data. To increase processing and storage efficiency, new [observations](../glossary.md#observation) for a datapoint are only stored when there is a \(significant\) change w.r.t. the previously observed/measured values. This is a common practice referred to as Change-of-Value \(CoV\). The preconfigured CoV threshold is 0.

Time series data can be stored at a maximum resolution of nanoseconds on aedifion.io. However, when ingesting data from an [automation network](../glossary.md#automation-network), the resolution is usually limited by the maximum sampling frequency that the [devices](https://docs.aedifion.io/docs/glossary#device) on the automation network support. Older BACnet devices, e.g., usually support sampling frequencies up to $$1/5s$$ while modern BACnet devices can often be sampled at $$1/s$$and faster. Sample rates can be flexibly adjusted per [project](../glossary.md#project), [device](../glossary.md#device), and even per [datapoint](../glossary.md#datapoint).

## Data provision

aedifion.io offers various ways of data provision, i.e., via a web frontend, via rest and streaming APIs, as well as via integrations into third party software.

### Frontend

The aedifion.io web frontend offers, among other features, search of data and meta data, different means of personalization, various flavors of plots, and export of data. A major revision with better user experience and more functionality is scheduled for mid/end 2019.

_Learn more? Explore the_ [_frontend guide_](frontend-1.md) _or visit our frontend at_ [_www.aedifion.io_](https://www.aedifion.io)_._

### HTTP API

All functionality of aedifion.io is exposed through the [HTTP API](../developers/api-documentation/). Thus, it covers all functionality of the frontend and more. The API can be used with various third party tools or called from virtually any modern programming language. At [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/), you can find a web environment for user‐friendly testing and compilation of API interactions. In addition, other retrieval methods can be provided for e.g. MATLAB upon request.

_Learn more? Explore our_ [_HTTP API tutorials_](../developers/api-documentation/guides-and-tutorials/) _or try out our API user interface at_ [_https://api.aedifion.io/ui/_](https://api.aedifion.io/ui/)_._

### MQTT API

The MQTT API offers a publish/subscribe model to stream data into and from the aedifion.io platform. You can subscribe to the data of your whole plant or limit your subscriptions to a few selected datapoints of interest.

_Learn more? Try out our_ [_MQTT API tutorials_](../developers/mqtt-api/guides-and-tutorials/)_._

### Third party software

We are integrating access to data on aedifion.io into a growing amount of third-party software such as Microsoft Excel or Grafana.

_Learn more? Explore the overview of_ [_existing integrations_](integrations.md)_._

## Data processing

A growing range of post-processing methods is continuously being built directly into the aedifion.io platform such as down sampling, interpolation, or different general-purpose statistical analyses. Beyond the built-in functionality, you can set up custom stream and batch processes and deploy virtual datapoints that run on your project's data.

{% hint style="success" %}
aedifion.io offers data processing in compliance with the German VDI 6041 "Technical Monitoring" and the international ISO 50 001 "Energy Management" standards.
{% endhint %}

### Stream

Stream processes cover use cases such as nominal-actual-comparisons as required in the German VDI 6041 "Technical Monitoring" standard. A stream process runs a calculation of a free-to-choose mathematical relationship on each new event/observation of a referred datapoint. Stream processes relate to one or more datapoints.

{% hint style="info" %}
Examples for stream processes: 

* Heat flow calculation: $$\dot{Q} = \dot{m} \cdot c_p \cdot (\vartheta_{out} - \vartheta_{in})$$
* Coefficient of performance: $$\eta = \frac{\dot{Q{th}}}{P_{el}}$$ 
* System sanity/operation checks: $$\text{actual value} \approx \text{expected value}$$ 
{% endhint %}

### Batch

Batch processes cover more complex calculations and account for the more complex use cases of the ISO 50 001 "Energy Management" standard. A batch process is not operated continuously - like stream processes - but on a certain trigger, e.g., a pre-set time or up on your request. This process can run complex calculation using high-volumes of historical data.

{% hint style="info" %}
Examples for batch processes: 

* Energy Efficiency Ratio \(heat pump\)
* Energy reporting over long periods 
* Control loop analysis
{% endhint %}

### Virtual datapoints

A virtual datapoint is a datapoint not gathered from a local plant but denotes a predefined stream or batch process that runs on one or multiple datapoints. The virtual datapoint makes the output continuously available in the form of a new datapoint in aedifion.io's time series database. 

You can choose from a continuously growing list of virtual datapoints engineered and provided by aedifion and apply them to your project. Of course, all alarming and notification paradigms that run on the original physical datapoints can also be used on such virtual datapoints. This allows you, e.g., to monitor and alert on complex relationships and conditions.

{% hint style="info" %}
Examples for virtual datapoints:

* Heat flux determination from temperature and volume flow sensors
* Compliance with operating rules, e.g. by correlating valve positions
* Determination of current KPI values like coefficients of performance
{% endhint %}

## Data management and structuring

Your building or plant can easily comprise thousands of datapoints and managing these can quickly become a complex task. Thus, aedifion.io offers different methods to manage and structure data as well as to enrich it with meta data.

### Favorites 

You can flag any datapoint as a favorite, e.g., to mark frequently inspected or important [datapoints](../glossary.md#datapoint). Using the frontend or API, you can filter the list of datapoints by favorites in order to quickly access them. 

### Tags

[Tags](../glossary.md#tag) are small pieces of meta data that are attached to [devices](../glossary.md#device) and [datapoints](../glossary.md#datapoint). Tags are automatically added by aedifion from the collected meta data as well as using artificial intelligence methods. Of course, users can freely add their own tags. Tags can then be used to filter devices and datapoints, to match analysis algorithms, or to build up structured naming schemes such as [Brick](https://brickschema.org/) or [BUDO](https://github.com/RWTH-EBC/BUDO).

### Datapoint keys

A [datapoint key](../glossary.md#datapointkey) is a datapoint naming schemes that groups alternate for all or some datapoints of a building under a common key. Alternate datapointkeys are required, e.g., to address needs of different datapoint naming schemes such as as [Brick](https://brickschema.org/) or [BUDO](https://github.com/RWTH-EBC/BUDO), logical uniqueness, or simply individual user preferences. 

_Learn more? Explore our_ [_tutorials on favorites, tagging, and renaming_](../developers/api-documentation/guides-and-tutorials/tagging.md)_._

## Project, user, and permission management

Each customer on aedifion.io is managed within his/her own realm such that data and meta data of that customer are always strictly separated from the resources of other customers. Each customer can have multiple projects on aedifion.io which correspond to administrative sub-realms within that company. Users can be freely added to the company and be flexibly assigned to or removed from the company's projects.

Access to all resources within a company and its projects is strictly controlled through a role-based access control \(RBAC\) mechanism which allows flexible and fine-grained rights management, e.g., you can restrict users' access to specific projects, to a subset of API endpoints, or even to single datapoints or tags. The RBAC system allows you to configure aedifion.io to meet your individual data privacy requirements.

_Learn more? Explore our_ [_administration tutorial_](../developers/api-documentation/guides-and-tutorials/administration.md)_._

## Alarming and notifications

aedifion.io has various alarming schemes built-in that you can deploy on projects and datapoints. Threshold alarms trigger alerts, e.g., when the CO2 concentration rises too high, while throughput alerts can be used to monitor the health of your sensors. Alerts are delivered through different alarming channels, such as web, email, instant messengers, or voice output using, e.g., Amazon's Alexa. This accounts for flexible, user-centric and innovative alarming functionalities.

![The aedifion.io alarming web frontend.](../.gitbook/assets/bildschirmfoto-2019-03-08-um-17.04.57.png)

_Learn more? Explore our_ [_alarming tutorial_](../developers/api-documentation/guides-and-tutorials/alarming.md)_._

## Integrations

We continuously build integrations of aedifion.io with and into various popular third services such as cloud providers, instant messengers, Amazon's Alexa, or 3D visualizations. These integrations augment aedifion's core services and allow you to chain aedifion.io with other services to build whole new creative and innovative service pipelines.

_Learn more? Read about the_ [_existing integrations_](integrations.md)_._

## Setpoint management and system overwrite

aedifion.io provides basic control functions whenever a datapoint is generally controllable in the field. This covers simple setpoint writing as well as manipulating local control loops or even overwriting local system output. Further, aedifion.io has a decent scheduling functionality that allows you to robustly execute control sequences on the aedifion edge device and monitor and control the execution from cloud as well as to even chain control to our integrations such as Alexa or chatbots. In extreme cases, local control hardware can be reduced to in-out-devices whereas all logic is operated in the cloud.

{% hint style="danger" %}
Safety of cloud-based controls is critical. aedifion.io offers various, locally executed, safety mechanisms that handle, e.g., connection loss, crashes, and human error.
{% endhint %}

### Deployment scenarios

Control algorithms can be operated in different ways. A cloud-operated algorithm uses the internet to communicate its control decisions or manipulated variables. Contrary, an edge-operated algorithm is executed locally on the aedifion.io edge device within the customer's control system and only requires Internet connectivity to receive commands and updates. An air-gapped deployment is a more classical set up, quite like a local programmable logic controller, but with a more advanced control runtime, offering the possibility to execute more complex controls, including optimizations and simulations.

{% hint style="success" %}
aedifion.io runs in cloud, edge, and air-gapped deployments.
{% endhint %}

## Authentication mechanisms

aedifion.io supports various \(single-sign on\) authentication methods, e.g., HTTP Basic Auth, OpenID Connect, OAuth 2.0, and SAML 2.0. aedifion.io can connect to existing user directories, e.g., LDAP and Active Directory, and supports different social logins, e.g., Google, Github, Facebook and the likes.

## Legal framework

aedifion.io's legal framework consists of a license agreement \(Nutzungsvertrag\) between the customer and aedifion with attachments. I.e. the aedifion Terms and Conditions \(AGBs\), a contract for the commissioned data processing \(Auftragsdatenverarbeitung, ADV\), and technical and organizational measures for data security \(technisch-organisatorische Maßnahmen, TOMs\).​

## Availability and quality assurance

aedifion continuously supervises aedifion.io's system health using internal realtime measurements along the whole data pipeline, i.e, from collection of data over its storage and processing all the way to the output of data and generated insights. 

aedifion.io is mirrored across two identical but completely independent deployments that work redundantly in parallel. All time series data is backed up on a weekly basis while backups of all meta data are created each night. Backups are kept for the whole duration of the project.



_Within the next subpage, we introduce the aedifion edge device in more detail._

