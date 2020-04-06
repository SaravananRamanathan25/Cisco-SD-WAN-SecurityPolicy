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
* In the POSTMAN, make sure you set the environment as "Cisco-SD-WAN-Environment" in the top right corner![SelectEnvDetails](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-SecurityPolicy/blob/master/Images/SelectEnvDetails-Postman.png)
* Go to Environment options and edit the vmanage, j_username, j_password and port details as per your own vmanage environment. **User account should have write permission for the feature "Policy Configuration" Feature.**![EditEnvDetails](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-SecurityPolicy/blob/master/Images/UpdateEnvDetails_Postman.png)
* First execute the API under "Authentication\Authentication". This will make sure that you have logged into the vManage for that session.
* Next execute the API under "SecurityPolicy\1.To Create List\1a.To Create Data Prefix List".
* Next execute the API under "SecurityPolicy\1.To Create List\1b.To Create Zone List (source)".
* Next execute the API under "SecurityPolicy\1.To Create List\1c.To Create Zone List (destination)".
* Next execute the API under "SecurityPolicy\2.To Get List Reference\2a.To Get Data Prefix List".
  * In the response payload, search with the list name and find its corresponding listId as shown below ![Get_DataPrefix](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-SecurityPolicy/blob/master/Images/Get_DataPrefix.png)
* Next execute the API under "SecurityPolicy\2.To Get List Reference\2b.To Get Zone List".
  * In the response payload, search with the list name and find its corresponding listId as shown below ![Get_Zone](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-SecurityPolicy/blob/master/Images/Get_Zone.png) 
  * Note: Get the listId for both source and destination Zones created as part of 1b and 1c steps.
* In order to execute the third API under "SecurityPolicy\3.To Create Policy Component\To Create Zone Based FW Policy Component", replace value for below parameter in the request body 
  * DataPrefix-listId : listId fetched from the API 2a
  * Zone(source)-listId : listId fetched from the API 2b
  * Zone(destination)-listId : listId fetched from the API 2b
  * After replacing above values, payload will somewhat look like as, ![Sample_PolicyComponent_Payload](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-SecurityPolicy/blob/master/Images/Sample_PolicyComponent_Payload.png)
* Next execute the API under "SecurityPolicy\4.To Get Definition Id of Policy Component\To Get Definition Id of Zone Based FW Policy Component".
  * In the response payload, search with the Policy Component name and find its definitionId as shown below ![Get_FW](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-SecurityPolicy/blob/master/Images/Get_FW.png)
* In order to execute the fifth API under "SecurityPolicy\5.To Create Security Policy\To Create Security Policy", replace value for below parameter in the request body 
  * PolicyComponent-definitionId : templateId fetched from the fourth API
  * After replacing above values, payload will somewhat look like as, ![Sample_Policy_Payload](https://github.com/SaravananRamanathan25/Cisco-SD-WAN-SecurityPolicy/blob/master/Images/Sample_Policy_Payload.png)
* We have successfully created a Security Policy.

# Security Policy Components' Cardinality with Security Policy
 
| Security Policy Lists | Cardinality for one instance of security policy | Lists that can be used | Important Aspects |
| ----------------------- | -------------------------------------------- | ---------------------- | ----------------- |
| Firewall | 0..many | <p>Data Prefix (Source/Desitination)<br>Zone (Soruce/Destination)<br><Application (Application/Application Family List </p> | 1. Variables are enabled only for the fields: Source IP Prefix and Destination IP Prefix<br>2. Application  Family list alone cannot be added in the sequence rule. If added, following error message appears: "Please add at least one of the remaining Match fields along with Application/Application Family List." |
| Intrusion Prevention | 0..1 | Signatures (Signature Whitelist) | Container/UTD Image (Container Profile (Security App Hosting) additional Template which is child of Security Policy Template should also be selected for this - while associating the security policy with device Template) | 
| URL Filtering | 0..1 | <p>Whitelist URLs (Whitelist URL List)<br>Blacklist URLs (Blacklist URL List)</p> | Container/UTD Image (Container Profile (Security App Hosting) additional Template which is child of Security Policy Template should also be selected for this - while associating the security policy with device Template) | 
| Advanced Malware Protection | 0..1 | <p>Thread Grid API Key (this is not in List.  This is separate custom option)<br>**Region<br>**Key</p> | Container/UTD Image (Container Profile (Security App Hosting) additional Template which is child of Security Policy Template should also be selected for this - while associating the security policy with device Template) |
| DNS Security | 0..1 | <p>Domain (Local Domain Bypass List)<br>Umbrella Registration (this is not in List.  This is separate custom option)<br>**Registration Token</p> | |


# Security Policy and its related list/component - Delete Operations Analysis

| Level | Analysis |
| ----------------------- | -------------------------------------------- |
|List (3rd level) | 1. Cannot be deleted if it is referenced in any 2nd level policy.<br>2. Will not be auto deleted if the 2nd level parent policy is deleted. It has to be deleted manually.|
|Individual Security Policy Lists (2nd Level)|1. Cannot be deleted if it is referenced in any 1st  level policy.<br>2. Will be auto deleted if all the 1st level parent policies are deleted.|
|Higher Level Security Policies (1st Level)|1. It will delete all the referred 2nd level policies when it get deleted.<br>2. However, if any of the 2nd level policy is referenced in any other 1st level policy, then that 2nd level policy will not be deleted.|
