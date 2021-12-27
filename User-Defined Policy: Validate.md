## User-Defined Policy: Validate

### Prerequisite
- This UDP only activates with the [extract-clientid-global-policy.yaml](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/extract-clientid-global-policy.yaml), because it uses variables that are sent in that code.

The user-defined policy (UDP) or custom policy [validate-kid-udp.zip](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/validate-kid-udp.zip), once deployed, will be available in the designer for developers to specify an Azure keystore endpoint to pull the public keys in order to validate the requestors Key-ID (KID) from their authorization token (base64 encoded JWT).  

NOTE: This is a specific IBM APIC User-defined policy example. Users may take this example and change the UDP code (inside the .zip) to extract any key to validate against. In this example, the Key-ID from a requestors token is validated against a list of public keys from what is returned from the URL specified in the Keystore URL of the custom policy.  

A sample of the response from the Keystore looks like the following:  
```
{
  "keys": [
    {
      "kty": "RSA",
      "use": "sig",
      "kid": "kid1",
      "x5t": "KXEg",
      "n": "oaTBQ",
      "e": "AQAB",
      "x5c": [
        "MIDw7R"
      ]
    },
    {
      "kty": "RSA",
      "use": "sig",
      "kid": "kid2",
      "x5t": "l3sQ-50cCH4xBVZLHTGwnSR7680",
      "n": "sfsXDJVAYkwb6xUQ",
      "e": "AQAB",
      "x5c": [
        "MI/vJ"
      ]
    },
    {
      "kty": "RSA",
      "use": "sig",
      "kid": "kid3",
      "x5t": "MoXW0",
      "n": "yritIQ",
      "e": "AQAB",
      "x5c": [
        "MIIy1skut"
      ]
    }
  ]
}
```  
Notice there are 3 kid key values in the response, which will be validated against what the kid value is sent from the request token. The udp code will reflect this validation logic after the code comment ``` Validate request JWT kid against MSKey KIDs ```. You may update the code as required to reflect your project.  

The policy within the APIC designer will look like the following:  
![image](https://user-images.githubusercontent.com/66093865/147147519-8daf4341-43f3-48bf-b253-2d764935816f.png)  

The purpose of this UDP is to:  
1.	Call a keystore url to store and cache the response on the gateway.  
2.	Take the requestors extracted KID from the extract-clientid-global-policy.yaml.  
3.	Then validate the requestors KID against the list of KIDs cached on the gateway, which originally retrieved from the an Azure keystore.  
4.	If no match is found, APIC will error with “KeyID Validation Error: The request KeyID was not found in the MS Key Store”.  
5.	If there is no response body from the Azure keystore, the UDP will presume it is down, and only use the client_id validation, which took place in the global policy, and resume the transaction only having validated the client_id and client_secret.  

## User-Defined Policy (Validate): Deploy, using, and removing  
It is best to deploy the UDP first to test because this will not disrupt the whole catalog. Ensure that the UDP is deployed to the sandbox as well as any additional catalogs you have in order to see the UDP in the api.  
Once the udp is deployed, you may test the Key ID validation portion of the solution and once that is satisfied, then you may deploy the Global policy to test the token decoding, and client ID extraction.  

### Deploying User-Defined Poklicy: Validate KID
Prerequisite:
Download the validate-kid-udp.zip if not done already: [validate-kid-udp.zip](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/validate-kid-udp.zip)  

Deploying to the Sandbox catalog:
1.	Navigate to the Sandbox catalog > Catalog Settings > Gateway Services, and click on the vertical ellipsis, selecting the View policies as shown in the diagram below  
![image](https://user-images.githubusercontent.com/66093865/147493985-cc16bca5-46ad-4635-9734-c991a1a9cb35.png)  

2.	Upload the UDP in the Policies section.  
![image](https://user-images.githubusercontent.com/66093865/147493988-e9280c15-c5db-4198-9afe-ad8320c8711e.png)  

###	Using User-Defined Policy: Validate KID  
1.	Once the policy is uploaded, you will see it in the API assembly under the User Defined section.  
NOTE: There is a known issue where the policy may not appear or fields within the policy may be missing. Please drag and drop another policy into the flow and remove it to see the custom policy reappear or appear.  
![image](https://user-images.githubusercontent.com/66093865/147494087-89578f52-fb5e-40ec-931f-9b4bdaf6f352.png)  

2.	Input the azure keystore endpoint to pull the public keys from and set the ttl desired.  
The url may be set to the catalog property as well.  
NOTE: The sample diagram below shows $(keystore-url) which you can set the actual url either in the api properties or catalog properties.  
![image](https://user-images.githubusercontent.com/66093865/147494118-8ccb21a7-ce06-409b-aadb-979ebf46d390.png)  

###	Remove User-Defined Policy: Validate KID
Ensure that the policies are deleted from the API and the API is saved without the UDP, then go back into the policy section of the catalog, and click on the ellipsis to find the Delete button.  
![image](https://user-images.githubusercontent.com/66093865/147494166-f0f81751-8f60-46d6-b83b-1d7eb946d4dc.png)  





