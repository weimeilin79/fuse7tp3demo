# Integrating existing SaaS API with Broker connection in Fuse Online

## Prerequisites
* A valid [Fuse Online](https://www.redhat.com/en/explore/fuse-online) account 
* Finished the simple SaaS integration with Swagger Document [demo](https://github.com/weimeilin79/fuse7tp3demo/blob/master/README.md) 

## Background

Utility payment management system has offered an online service to pay for the water utility bill -- Water Company. You job is kick off a payment whenever an event is sent to the default message broker. 
 
![Demo 50](pic/demo50.png)

The **Water Company** API has a backend dashboard that display all the payment. You will be able to view the result of the payment. 
(Data cleans every three hours.)

> https://water-company-tp3demo.4b63.pro-ap-southeast-2.openshiftapps.com/main

![Demo 37](pic/demo37.png)

OpenShift managment Console is avaliable at:

> https://console.fuse-ignite.openshift.com/console/

![Demo 51](pic/demo51.png)

## Low Code Integration

### Step One - Start and configure AMQ broker

Before we start integration, let's start the default broker. Go to the OpenShift managment Console. And select projecxt on *Fuse Online*

> https://console.fuse-ignite.openshift.com/console/

![Demo 52](pic/demo52.png)

Scale up **broker-amq** from 0 to 1 located at the buttom of the overview.
![Demo 53](pic/demo53.png)

Once started, click on the running pod.
![Demo 55](pic/demo55.png)

Go to AMQ console by click on the *Open Java Console*
![Demo 54](pic/demo54.png)

Click on +Create on the tab panel
![Demo 56](pic/demo56.png)

Enter 
> **Queue name:** paymentevent
> 
> **Destination Type:** Queue
 
![Demo 57](pic/demo57.png)

On the lefthand panel, select Queue with name paymentevent and click **+Send** on the top tab panel. and set the credentail of the sender by clicking on **Preference** .

![Demo 58](pic/demo58.png)

Start config
> **Activemq user name:** amq
> 
> **Activemq password:** topSecret
And click done on the left panel.

![Demo 59](pic/demo59.png)


Go back to the Ignite console. in navigation panel, click Connections, and click on *Create Connection*
![Demo 60](pic/demo60.png)

Selects "AMQ" as the Connection Type.
![Demo 61](pic/demo61.png)

Enter

> **Base URL:** tcp://broker-amq-tcp:61616
> 
> **User Name:** amq
> 
> **Password**: topSecret

and select *"Next"*
![Demo 62](pic/demo62.png)

Click on validate.
![Demo 63](pic/demo63.png)

Name your connection.
![Demo 64](pic/demo64.png)
![Demo 65](pic/demo65.png)

### Step Two - Integration

In the Ignite navigation panel, click Integrations, and click on *Create Integration*

![Demo 15](pic/demo15.png)

On the Choose a Start Connection page, click the default DefaultBroker connection.

![Demo 66](pic/demo66.png)

Choose *Subscribe for message* to listent to events send to broker.
![Demo 67](pic/demo67.png)

Enter following input. 

* **Destination Name:** *paymentevent*
* **Destination Type:** *Queue*

![Demo 68](pic/demo68.png)

On the Choose a Finish Connection page, click **PayBill** connection that you created.
![Demo 69](pic/demo69.png)

On the Choose an Action page, click **Payment**, which will kick off water utility bill payment.
![Demo 70](pic/demo70.png)

In the left panel, hover over the plus sign between the TODO *PERIODIC SQL INVOCATION* step and the finish *PAYMENT* connection to display a pop-up in which you click Add a Step.
![Demo 71](pic/demo71.png)

On the Choose a Step page, click Data Mapper. 
![Demo 72](pic/demo72.png)

In the data mapper, the Sources panel on the left displays the fields in the output from the Todo step. The Target panel on the right displays the fields from the Water Company API. In the Target panel, expand the body field. click on the '+ ' sign in Constant. 

![Demo 73](pic/demo73.png)

Create two constant

* **50:** *String*
* **YOUR_NAME:** *String*
![Demo 75](pic/demo75.png)

Map your contant from Source panel to Target accordingly

![Demo 76](pic/demo76.png)

Name your integration and click publish.
![Demo 77](pic/demo77.png)

![Demo 78](pic/demo78.png)


### Step Three - Integration Result

Go back to your AMQ cosole, enter pay in the payload and click send message.

![Demo 79](pic/demo79.png)
![Demo 80](pic/demo80.png)


Go to the *Water Company* backend dashboard and view the result of the integration. 
> https://water-company-tp3demo.4b63.pro-ap-southeast-2.openshiftapps.com/main
![Demo 81](pic/demo81.png)



## Resource
[Fuse Online] (https://www.redhat.com/en/explore/fuse-online) - https://www.redhat.com/en/explore/fuse-online
[Demo Video](https://vimeo.com/257550267) - https://vimeo.com/257550267
[Demo Video](https://vimeo.com/257656566) - https://vimeo.com/257656566