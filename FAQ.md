### Q: How does Inferno test US Core Encounter resource?
**A:** Inferno Encounter test is only concerned with references to Encounter resources that it knows MUST support the US Core Encounter profile. And it turns out that not all references to Encounter resources within other US Core profiles need to conform to the US Core Encounter profile. For an example of a reference to `Reference(US Core Encounter)`, see [US Core DiagnosticReport Note](http://hl7.org/fhir/us/core/STU3.1.1/StructureDefinition-us-core-diagnosticreport-note.html)'s `encounter` element. [US Core DocumentReference](http://hl7.org/fhir/us/core/STU3.1.1/StructureDefinition-us-core-documentreference.html)'s `context.encounter` is another example. If you place encounter references in one of those spots, you should start getting references that are tested in the Encounter test. Note: US Core has two profiles for DiagnosticReport. Only the DiagnosticReport Note profile mentioned above has US Core Encounter reference. The other profile (US Core DiangosticReport Lab Result) does NOT have US Core Encounter reference.

### Q: What is the difference between "skipped" test and "omitted" test?
**A:** Inferno has four states for test result:
| State | Meaning |
|---|---|
| Pass | Server response is valid for this test. |
| Omit | This test does not apply to the server due to server configuration. The pass or fail of a test sequence is not affected by omitted test. |
| Skip | Server response doesn't not contain all information, and Inferno can NOT complete the test to verify server's behavior. Tester should provide additional data to continue the test. A test sequence is treated as failed if there is skipped test. |
| Fail | Server response is NOT valid for this test. |

### Q: How does Inferno test MustSupport flag on an element?
**A:** Inferno follows guidance provided by HL7 FHIR [Conformance Rules](https://www.hl7.org/fhir/conformance-rules.html#mustSupport) and US-Core IG [General Guidance](http://hl7.org/fhir/us/core/STU3.1.1/general-guidance.html#must-support). Inferno checks that a server implementation SHALL demonstrate that it supports the "MustSupport" element in a meaningful way. In general, Inferno MustSupport test checks that among all resources in server response, if there is at least one resource has this "MustSupport" element populated. It is not required to have one resource having all MustSupport elements populated. Using "Data Absent Reason" (DAR) extension on a "MustSupport" element does NOT count as a "meaningful way" so Inferno count "MustSupport" element with DAR extension.