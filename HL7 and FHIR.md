# HL7 - FHIR mapping
- https://build.fhir.org/ig/HL7/v2-to-fhir/ConceptMap-segment-pv1-to-encounter.html
- 
# HL7 segment definitions
- https://v2plus.hl7.org/2021Jan/segment-definitions.html
- [CodeSystem: eventType](https://terminology.hl7.org/4.0.0/CodeSystem-v2-0003.html)


# FHIR and Database
- https://www.youtube.com/watch?v=c-I04rtBZLc&list=PLsR-zcO--dypUxuALrmuq70aM-VGX_ql1&index=26&t=734s
- 

# azure api for fhir  
https://www.youtube.com/watch?v=cggfZYZtUzE  
http://hl7.org/fhir/R4/administration-module.html   
https://github.com/nazrulworld/fhir.resources/blob/main/fhir/resources/encounter.py  
https://github.com/microsoft/fhir-server/blob/main/docs/QuickstartDeployPortal.md  
https://learn.microsoft.com/en-us/azure/healthcare-apis/fhir/using-curl?tabs=CLI  


# FHIR Message: 
```
# an example:
<Bundle xmlns="http://hl7.org/fhir">
  <id value="urn:uuid:77831928-2a35-4c08-9496-8232323bf48c"/>
  <!-- normal bundle stuff -->
  <entry>
    <fullUrl value="urn:uuid:6080d4a7-5e05-45dc-96d5-f75329564d1f"/>
    <resource>
      <MessageHeader>
			  <id value="cac8143e-6138-4f45-b086-bb8ebf976aae">
        <!-- normal message header stuff -->
        <event>
          <system value="urn:ietf:rfc:3986"/>
          <!-- value set expansion -->
          <code value="http://hl7.org/fhir/OperationDefinition/ValueSet-expand"/>
        </event>
        <!-- more normal message header stuff -->
        <data>
          <reference value="urn:uuid:00213637-dc7c-40d2-a7de-f4ef1eea5685"/>
        </data>
      </MessageHeader>
    </resource>
  </entry>
  <entry>
    <fullUrl value="urn:uuid:00213637-dc7c-40d2-a7de-f4ef1eea5685"/>
    <resource>
      <Parameters>
        <parameter>
          <name value="identifier"/>
          <valueUri value="http://hl7.org/fhir/ValueSet/identifier-type"/>
        </parameter>
      </Parameters>
    </resource>
  </entry>
</Bundle>
```
- http://www.hl7.org/implement/standards/fhir/messaging.html


# Reference:   
- [HL7 Segment definitions](https://v2plus.hl7.org/2021Jan/segment-definitions.html)
- [python-hl7 - Easy HL7 v2.x Parsing](https://python-hl7.readthedocs.io/en/latest/index.html)

- https://learn.microsoft.com/en-us/azure/healthcare-apis/fhir/overview


