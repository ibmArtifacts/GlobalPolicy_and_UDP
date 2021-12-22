## User-Defined Policy: Validate

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
2.	Base64 decode the requestors JWT and extract the KID from the payload.  
3.	Then validate the requestors KID against the list of KIDs cached on the gateway, which originally retrieved from the an Azure keystore.  
4.	If no match is found, APIC will error with “KeyID Validation Error: The request KeyID was not found in the MS Key Store”.  
5.	If there is no response body from the Azure keystore, the UDP will presume it is down, and only use the client_id validation, which took place in the global policy, and resume the transaction only having validated the client_id and client_secret.  

