These artifacts are for APIC v10.  

The artifacts showcase a specific sample for [APIC Global Policies](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=applications-working-global-policies) and [APIC User-Defined Policies (Custom Policies)](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=authoring-policies).  

## Global Policy: Decode JWT & Extract client_id
The [extract-clientid-global-policy.yaml](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/extract-clientid-global-policy.yaml) APIC Global Policy is a pre-flow policy, which will:
1. Take the requestor's inbound JWT and Base64 decode the Bearer <jwt>.
2. Extract the apicid and kid value from the decoded JWT.
3. Then set the apicid to the client_id header for downstream processing on the APIC API, and set kid value to a variable to be used downstream as well.  

The same API must be able to handle both:  
-	An authorization header (token) validation, which steps 1 through 3 processes to validate the client_id and the custom policy will validate the Key-Id in the token.  
or  
-	(Internal API callers) a client_id and client_secret validation will take place without key-id validation.  
  
NOTE: The inbound client_id and client_secret field can be set to something different, like X-IBM-Client-Id, clientid, or etc., but it has to be updated in the extract-clientid-global-policy.yaml.  

