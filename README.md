
# Anypoint Template: Salesforce and Microsoft Dynamics CRM Contact Aggregation

![d731d9a5-62fd-4adf-9eae-cc099580fb7c-image.png](https://exchange2-file-upload-service-kprod.s3.us-east-1.amazonaws.com:443/d731d9a5-62fd-4adf-9eae-cc099580fb7c-image.png)

# License Agreement
This template is subject to the conditions of the 
<a href="https://s3.amazonaws.com/templates-examples/AnypointTemplateLicense.pdf">MuleSoft License Agreement</a>.
Review the terms of the license before downloading and using this template. You can use this template for free 
with the Mule Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case
As an admin I want to aggregate contacts from Salesforce and MS Dynamics instances and compare them to see which contacts can only be found in one of the two and which contacts are in both instances. 

This template generates its result as a CSV report that is sent by email.

This template serves as a foundation for extracting data from two systems, aggregating data, comparing values of fields for the objects, and generating a report on the differences. 

As implemented, it gets contacts from Salesforce and Microsoft Dynamics, compares by the email address of the contacts, and generates a CSV file that shows contact names in Salesforce, contact names in MS Dynamics, and the email and contacts IDs in Salesforce and MS Dynamics. The report is sent by the email to a configured group of email addresses.

# Considerations

To make this template run, there are certain preconditions that must be considered. All of them deal with the preparations in both, that must be made for the template to run smoothly. Failing to do so could lead to unexpected behavior of the template.

## Salesforce Considerations

Here's what you need to know about Salesforce to get this template to work:

- Where can I check that the field configuration for my Salesforce instance is the right one? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US">Salesforce: Checking Field Accessibility for a Particular Field</a>
- Can I modify the Field Access Settings? How? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US">Salesforce: Modifying Field Access Settings</a>

### As a Data Source

If the user who configured the template for the source system does not have at least *read only* permissions for the fields that are fetched, then an *InvalidFieldFault* API fault displays.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault 
[ApiFault  exceptionCode='INVALID_FIELD'
exceptionMessage='
Account.Phone, Account.Rating, Account.RecordTypeId, Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are attempting to use a 
custom field, be sure to append the '__c' after the custom field name. Reference your 
WSDL or the describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```

## Microsoft Dynamics CRM Considerations


### As a Data Destination

There are no considerations with using Microsoft Dynamics CRM as a data destination.



# Run it!
Simple steps to get MS Dynamics and Salesforce Contact Aggregation running.


## Running On Premises
After this, to trigger the use case you just need to hit the local HTTP endpoint with the port you configured in the src/main/resources/common.properties file. If this is, for instance, `9090` then browse to `http://localhost:9090/generatereport` which creates a CSV report and sends it to the mails set.


### Where to Download Anypoint Studio and the Mule Runtime
If you are a newcomer to Mule, here is where to get the tools.

+ [Download Anypoint Studio](https://www.mulesoft.com/platform/studio)
+ [Download Mule runtime](https://www.mulesoft.com/lp/dl/mule-esb-enterprise)


### Importing a Template into Studio
In Studio, click the Exchange X icon in the upper left of the taskbar, log in with your
Anypoint Platform credentials, search for the template, and click **Open**.


### Running on Studio
After you import your template into Anypoint Studio, follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources.
+ Complete all the properties required as per the examples in the "Properties to Configure" section.
+ Right click the template project folder.
+ Hover your mouse over `Run as`
+ Click `Mule Application (configure)`
+ Inside the dialog, select Environment and set the variable `mule.env` to the value `dev`
+ Click `Run`


### Running on Mule Standalone
Complete all properties in one of the property files, for example in mule.prod.properties and run your app with the corresponding environment variable. To follow the example, this is `mule.env=prod`. 


## Running on CloudHub
While creating your application on CloudHub (or you can do it later as a next step), go to Runtime Manager > Manage Application > Properties to set the environment variables listed in "Properties to Configure" as well as the **mule.env**.
Once your app is all set and started, supposing you choose as domain name `msdynandsfdccontactaggregation` to trigger the use case you just need to hit `http://msdynandsfdccontactaggregation.cloudhub.io/generatereport` and the report is sent to the emails configured.

## Properties to Configure
To use this template, configure properties (credentials, configurations, etc.) in the properties file or in CloudHub from Runtime Manager > Manage Application > Properties. The sections that follow list example values.

### Application Configuration

**HTTP Connector configuration**   
+ http.port `9090` 

**Salesforce Connector configuration**
+ sfdc.username `bob.dylan@orga`
+ sfdc.password `DylanPassword123`
+ sfdc.securityToken `avsfwCUl7apQs56Xq2AKi3X`

**MS Dynamics Connector configuration**
+ msdyn.user `user@yourorg.onmicrosoft.com`
+ msdyn.password `yourPassword`
+ msdyn.url `https://yourorg.api.crm4.dynamics.com/XRMServices/2011/Organization.svc`
+ msdyn.retries `2`

**SMTP Services configuration**
+ smtp.host `smtp.gmail.com`
+ smtp.port `587`
+ smtp.user `exampleuser@gmail.com`
+ smtp.password `ExamplePassword456`

**Mail details**
+ mail.from `exampleuser1@gmail.com`
+ mail.to `exampleuser2@gmail.com`
+ mail.subject `SFDC Contacts Report`
+ mail.body `Contacts report comparing contacts from SFDC Accounts`
+ attachment.name `report.csv`

# API Calls
Salesforce imposes limits on the number of API Calls that can be made. However, we make API call to Salesforce only once during aggregation.


# Customize It!
This brief guide intends to give a high level idea of how this template is built and how you can change it according to your needs.
As Mule applications are based on XML files, this page describes the XML files used with this template.

More files are available such as test classes and Mule application files, but to keep it simple, we focus on these XML files:

* config.xml
* businessLogic.xml
* endpoints.xml
* errorHandling.xml


## config.xml
Configuration for connectors and configuration properties are set in this file. Even change the configuration here, all parameters that can be modified are in properties file, which is the recommended place to make your changes. However if you want to do core changes to the logic, you need to modify this file.

In the Studio visual editor, the properties are on the *Global Element* tab.


## businessLogic.xml
Functional aspect of the template is implemented on this XML, directed by one flow responsible of conducting the aggregation of data, comparing records and finally formatting the output, in this case being a report.
        
Using the Scatter-Gather component we query the data in different systems. After that the aggregation is implemented in DataWeave 2 script using Transform component.

Aggregated results are sorted as:

1. Accounts only in Salesforce
2. Accounts only in MS Dynamics
3. Accounts in both Salesforce and MS Dynamics

These are transformed to CSV format. The final report in CSV format is sent to the email addresses that you configured in mule.*.properties file.


## endpoints.xml
This is the file where you find the endpoint to start the aggregation. This template has an HTTP Listener as the way to trigger the use case.

### Trigger Flow
**HTTP Inbound Endpoint** - Start Report Generation
+ `${http.port}` is set as a property to be defined either on a property file or in CloudHub environment variables.
+ The path configured by default is `generatereport` and you are free to change for the one you prefer.
+ The host name for all endpoints in your CloudHub configuration should be defined as `localhost`. CloudHub then routes requests from your application domain URL to the endpoint.

## errorHandling.xml
This is the right place to handle how your integration reacts depending on the different exceptions. 
This file provides error handling that is referenced by the main flow in the business logic.

