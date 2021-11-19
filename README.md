These artifacts are for APIC v10.  

The artifacts showcase a specific sample for [APIC Global Policies](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=applications-working-global-policies) and [APIC User-Defined Policies (Custom Policies)](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=authoring-policies).  

## Global Policy: Decode JWT & Extract client_id
The [extract-clientid-global-policy.yaml](https://github.com/ibmArtifacts/GlobalPolicy_and_UDP/blob/main/extract-clientid-global-policy.yaml) APIC Global Policy is a pre-flow policy, which will:
1. Take the requestor's inbound JWT and Base64 decode the Bearer <jwt>.
2. Extract the apicid and kid value from the decoded JWT.
3. Then set the apicid to the client_id header for downstream processing on the APIC API, and set kid value to a variable
