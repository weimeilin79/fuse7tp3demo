#Simple SaaS integration with Swagger Document

##Prerequisites
* A valid [Fuse Online](https://www.redhat.com/en/explore/fuse-online) account 

## Background

Utility payment management system has offered an online service to pay for the water utility bill -- Water Company. You job is read from the input of a frontend application and setup re-occurring payment for different account. 
 
![Demo 1](pic/demo01.png)

To access the frontend application go to 
> **https://todo-*YOUR_FUSE_IGNITE_URL*/index.php**

![Demo 2](pic/demo02.png)

This data are stored in the default embedded Fuse Online database. 

![Demo 3](pic/demo03.png)

The **Water Company** API has a backend dashboard that display all the payment. You will be able to view the result of the payment. 
(Data cleans every three hours.)
https://water-company-tp3demo.4b63.pro-ap-southeast-2.openshiftapps.com/main

![Demo 37](pic/demo037.png)

## Low Code Integration

### Step One - Setup Connector and Connection

In the Ignite navigation panel, click Customizations, and click on *Create API Connector*

![Demo 4](pic/demo04.png) 

Click *Use a URL* and enter the swagger location

> https://raw.githubusercontent.com/weimeilin79/fuse7tp3demo/master/waterpayment.yml
  
![Demo 5](pic/demo05.png) 

Follow the instruction and click through to the final step. 
![Demo 6](pic/demo06.png) 
![Demo 7](pic/demo07.png) 
![Demo 8](pic/demo08.png) 
![Demo 9](pic/demo09.png)

In the Ignite navigation panel, click Connections, and click on *Create Connection*

![Demo 10](pic/demo10.png)

Selects "Pay Water Bill" as the Connection Type.
![Demo 11](pic/demo11.png)

Enter "/" as the Base path, and select *"Next"*
![Demo 12](pic/demo12.png)

Name your Connection.
![Demo 13](pic/demo13.png)
![Demo 14](pic/demo14.png)


### Step Two - Integration

In the Ignite navigation panel, click Integrations, and click on *Create Integration*

![Demo 15](pic/demo15.png)

On the Choose a Start Connection page, click the default PostgresDB connection.

![Demo 16](pic/demo16.png)

Choose *Periodic SQL Invocation* to setup the re-occurr payment.
![Demo 17](pic/demo17.png)

Enter following input. 

* **SQL Statement:** *SELECT TASK FROM TODO WHERE TASK LIKE 'reoccurring%';*
* **Period:** *30000*

![Demo 18](pic/demo18.png)

On the Choose a Finish Connection page, click **PayBill** connection that you created.
![Demo 19](pic/demo19.png)

On the Choose an Action page, click **Payment**, which will kick off water utility bill payment.
![Demo 20](pic/demo20.png)

In the left panel, hover over the plus sign between the TODO *PERIODIC SQL INVOCATION* step and the finish *PAYMENT* connection to display a pop-up in which you click Add a Step.
![Demo 21](pic/demo21.png)

On the Choose a Step page, click Data Mapper. 
![Demo 22](pic/demo22.png)

In the data mapper, the Sources panel on the left displays the fields in the output from the Todo step. The Target panel on the right displays the fields from the Water Company API. In the Target panel, expand the body field. Drag the **Task** field from Souce panel to the **senderid** in the Target Panel.

![Demo 23](pic/demo23.png)

On the Mappiung Detail Panel, Under Action, select **Separate**.

![Demo 25](pic/demo25.png)

On the same Mappiung Detail Panel, Under Targets, enter 2 in the Separate Index, and select *Add Transformation* and select **Trim**. And Click on *Add Target*

![Demo 26](pic/demo26.png)

Enter **amount** in the new popup target section. Make sure the index is set to **3**

![Demo 27](pic/demo27.png)

Click Done. 
![Demo 28](pic/demo28.png)

Click on *Save as Draft* to save this integration. And name the integration. Click on *Publish* to start the integration.
![Demo 29](pic/demo29.png)

Wait until the integration become active.
![Demo 30](pic/demo30.png)


### Step Three - Integration Result

Go to the frontend TODO application

> **https://todo-*YOUR_FUSE_IGNITE_URL*/index.php**

Enter *reoccurring YOUR_NAME BILLING_AMT* in the TODO form.

![Demo 31](pic/demo31.png)

Go to the *Water Company* backend dashboard and view the result of the integration. 
> https://water-company-tp3demo.4b63.pro-ap-southeast-2.openshiftapps.com/main

![Demo 32](pic/demo32.png)

Enter another name *reoccurring YOUR_NAME BILLING_AMT* in the TODO form.
![Demo 33](pic/demo33.png)

Go to the *Water Company* backend dashboard and view the result of the integration. 
> https://water-company-tp3demo.4b63.pro-ap-southeast-2.openshiftapps.com/main

![Demo 34](pic/demo34.png)

Delete all entry in frontend TODO application
![Demo 35](pic/demo35.png)

Deactivate the integration.
![Demo 36](pic/demo36.png)