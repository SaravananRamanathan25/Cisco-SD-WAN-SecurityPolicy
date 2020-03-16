# Cisco SD-WAN - SecurityPolicy (REST API)
The Cisco SD-WAN Solution (a.k.a Viptela) is a cloud-delivered overlay SD-WAN architecture that facilitates digital and cloud transformation for enterprises and communication service providers (CSP). It significantly reduces WAN costs and time to deploy new services.

Cisco SD-WAN builds a API based architecture that's crucial for enterprises and CSPs to do the configuration automation through their internal tools.

Once below templates are successfully created,  these can be grouped under one device template which can then attached to an edge device

* Feature Templates
* Local Policy Template
* Security Policy Template

# SecurityPolicy - POSTMAN Collection
This postman collection provides some example of various APIs dealing with SecurityPolicies.

This POSTMAN environment and collection that can be used to interact with the Cisco SD-WAN powered by Viptela vManage REST API. You can edit the variables in the environment to point to your own vManage instance. The collection contains REST API calls to create list, policy component and Security policy. Please note that the username should have write permission for "Policy Configuration" Feature for the usergroup that it is associated with.

# Steps to execute APIs in the Postman Collection
* Clone or Download the JSON files "CiscoSD-WAN-LocalPolicy.postman_collection.json" and "Cisco-SD-WAN-Environment.postman_environment.json"  
* Import above files to the POSTMAN  
* In the POSTMAN, make sure you set the environment as "Cisco-SD-WAN-Environment" in the top right corner![SelectEnvDetails](https://github.com/SaravananRamanathan25/....png)
* Go to Environment options and edit the vmanage, j_username, j_password and port details as per your own vmanage environment![EditEnvDetails](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-Feature-Templates/blob/master/Images/UpdateEnvDetails_Postman.png)
* First execute the API under "SecurityPolicy\1.To Create List\1a.To Create Data Prefix List".
* Next execute the API under "SecurityPolicy\1.To Create List\1b.To Create Zone List".
* Next execute the API under "SecurityPolicy\1.To Create List\1c.To Create Local App List".
* Next execute the API under "SecurityPolicy\2.To Get List Reference\2a.To Get Data Prefix List".
  * In the response payload, search with the list name and find its corresponding listId
* Next execute the API under "SecurityPolicy\2.To Get List Reference\2b.To Get Zone List".
  * In the response payload, search with the list name and find its corresponding listId
* Next execute the API under "SecurityPolicy\2.To Get List Reference\2c.To Get Local App List".
  * In the response payload, search with the list name and find its corresponding app name
* In order to execute the third API under "SecurityPolicy\3.To Create Policy Component\To Create Zone Based FW Policy Component", replace value for below parameter in the request body 
  * sourceZone : listId fetched from the API 2b
  * destinationZone : listId fetched from the API 2b
  * ref : listId fetched from the API 2a
* Next execute the API under "SecurityPolicy\4.To Get Definition Id of Policy Component\To Get Definition Id of Zone Based FW Policy Component".
  * In the response payload, search with the Policy Component name and find its definitionId
* In order to execute the fifth API under "SecurityPolicy\5.To Create Security Policy\To Create Security Policy", replace value for below parameter in the request body 
  * definitionId : templateId fetched from the fourth API
* We have successfully created a Security Policy.

| Local Policy Components | Cardinality for one instance of Local policy | Lists that can be used | Important Aspects |
| ----------------------- | -------------------------------------------- | ---------------------- | ----------------- |
| QoS Map | 0..many | Class Map (Queue) |     |
| Rewrite Rule | 0..many | Class Map (Class)      |      | 
| ACL | 0..many | <p>Class Map (Match/Actions: Class)<br>DataPrefix (Match: Source/Destination IP Prefix)<br>Policer (Actions: Policer)<br>Mirror (Actions: Mirror)</p> | <ul><li>Variables are enabled only for the fields, Source IP Prefix and Destination IP Prefix</li><li>Single ACL Policy can have 0..many ACL Sequence</li><li>Single ACL Sequence can have 1..many Sequence Rules</li></ul> |
| Route Policy | 0..many | <p>AS Path (Match: AS Path)<br>Community (Match: Community)<br>Extended Community (Match: Extended Community)<br>Prefix (Match: Address)</p> | <ul><li>Single Route Policy can have 0..many Sequence Types</li><li>Single Sequence Type can have 1..many Sequence Rules</li></ul> |	
