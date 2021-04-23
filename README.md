# Call ABAP Function Modules with SAP Data intelligence

##Introduction:
In this new blog we (myself + Michael Sun) want to describe the usage of performing ABAP function module calls with SAP Data Intelligence using Custom ABAP Operators. 
You may have already seen a similar blog post from Britta Thoelking about how to call function modules with pipelines in SAP Data Hub: 
https://blogs.sap.com/2019/11/04/abap-integration-calling-a-abap-function-module-within-a-sap-data-hub-pipeline/ 

The goal of this new blog post is to provide simplified guidance and a foundation for the general usage of function modules in Data Intelligence Pipelines that require certain input parameters, such as elements, structures or table, to execute the function module call. Be aware that the required input parameters in form of structures and tables can be also deeply nested into certain sublevels depending on the function module you choose.
One major part of this blog is to provide a generic Custom ABAP Operator that is extracting the metadata (or schema definition) of any ABAP function module that resides in your ABAP system that you connect to Data Intelligence. This operator will extract the required input parameters to prepare the execution of the function module and provide it in a JSON format as output. In the output JSON file all required input parameters are listed that the function module requires to successfully trigger the execution. This JSON is then being used as a template to define to map the required input parameters, which are then being used for the second part of this blog to use another custom ABAP operator that is actually executing the function module on the connected ABAP system. 

##Pre-requisites
The example illustrated in this blog is based on SAP S/4 HANA System Version 2020 as well as a Data Intelligence Cloud system version 2013. The general pre-requisites for connecting an SAP ABAP system with Data Intelligence are documented in the following link, but in general you need to have one of the following systems available:
-	S/4 HANA 1909 or higher (ABAP Pipeline Engine is already pre-installed)
-	SAP ECC with DMIS 2011 SP17 or DMIS2018 SP02 or later (depending on SAP release)
-	SAP BW with DMIS 2011 SP17 or later
In addition, you need to create a RFC-based connection of type “ABAP” in Data Intelligence against your  SAP ABAP system. In this blog we will not go into details for setting up the connection and expect the connection is already in place. Further details about setting up the connection can be found using the following links:
-	ABAP Connection Type for Data Intelligence: https://launchpad.support.sap.com/#/notes/2835207 

-	Data Intelligence ABAP Security Settings incl. whitelisting: https://launchpad.support.sap.com/#/notes/2831756

-	Note Analyzer for ABAP-based Replication Technology:
https://launchpad.support.sap.com/#/notes/2596411 
( Using  Scenario “ABAP Integration for SAP Data Intelligence”)

##Use Case:
The use case we focus on is using two pipelines in the Data Intelligence Modeler by using an existing function module to create sales orders in a remote S/4 HANA system. The first pipeline will leverage a custom ABAP operator to extract the metadata of the function module including the required input parameters for performing the function module call. 
In a second pipeline, we will perform the required mapping for the generated JSON file and provide the required input parameters to trigger the function module call in the connected ABAP system with a second custom ABAP operator. For simplification reasons, we perform a  manual mapping of the required input parameters in a Go operator, but in real scenarios you may perform the mapping based on data coming from another connected system, a web service or from a CSV file. In this blog we will use “BAPI_EPM_SO_CREATE” to create a sales order in our S/4 HANA system that requires certain input parameters for creating a sales order.

##How to obtain support
This is an example code without any commitment for getting support.
