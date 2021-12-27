These artifacts are for APIC v10 and v2018.  

The artifacts showcase specific samples for [IBM API Connect Global Policies](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=applications-working-global-policies) and [IBM API Connect User-Defined Policies (Custom Policies)](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=authoring-policies). 2 separate policies.  

## Global Policy: Decode JWT & Extract client_id
The [extract-clientid-global-policy.yaml](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/extract-clientid-global-policy.yaml) APIC Global Policy is a pre-flow policy, which will:
1. Take the requestor's inbound JWT and Base64 decode the Bearer <jwt>.
2. Extract the apicid and kid (key-ID) value from the decoded JWT.
3. Then set the apicid to the client_id header for downstream processing on the APIC API, and set kid (Key-ID) value to a variable to be used downstream as well.  

## User-Defined Policy: Validate Key ID (KID) 
The [validate-kid-udp.zip](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/validate-kid-udp.zip) APIC User-Defined Policy is an Assemble policy, which each API requiring this specific security mechinism will use, which will:
1. Take the set KID variable from the extract-clientid-global-policy (the clientid and key-id) and validate the list of KIDs from the response of the keystore which URL points to.
  
----
The global policy and custom policy allows for an API (published on IBM API Connect) capable of handing both:  
-	An authorization header (token) validation: to validate the client_id (from the global policy) and validate the Key-Id in the token (from the custom policy).  
or  
-	(Internal API callers) a client_id and client_secret validation will take place without key-id validation.  
  
NOTE: The inbound client_id and client_secret field can be set to something different, like X-IBM-Client-Id, clientid, or etc., but it has to be updated in the extract-clientid-global-policy.yaml.  
  
----  
Next:  
  
[User-Defined Policy: Validate KID --> Deploy, use, and remove](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/User-Defined%20Policy:%20Validate.md)  
  
[Global Policy: Decode JWT and Extract client_ID --> Deploy, and remove](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/Global%20Policy:%20Decode%20JWT%20and%20Extract%20client_id.md)  
  
  

