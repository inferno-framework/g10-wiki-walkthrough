#### Q: How does Inferno test US Core Encounter resource?
**A:** Inferno Encounter test is only concerned with references to Encounter resources that it knows MUST support the US Core Encounter profile. And it turns out that not all references to Encounter resources within other US Core profiles need to conform to the US Core Encounter profile. For an example of a reference to `Reference(US Core Encounter)`, see [US Core DiagnosticReport Note](http://hl7.org/fhir/us/core/STU3.1.1/StructureDefinition-us-core-diagnosticreport-note.html)'s `encounter` element. [US Core DocumentReference](http://hl7.org/fhir/us/core/STU3.1.1/StructureDefinition-us-core-documentreference.html)'s `context.encounter` is another example. If you place encounter references in one of those spots, you should start getting references that are tested in the Encounter test. Note: US Core has two profiles for DiagnosticReport. Only the DiagnosticReport Note profile mentioned above has US Core Encounter reference. The other profile (US Core DiangosticReport Lab Result) does NOT have US Core Encounter reference.

#### Q: What is the difference between "skipped" test and "omitted" test
**A:** Inferno has four states for test result:
| State | Meaning |
|---|---|
| Pass | Server response is valid for this test. |
| Omit | This test does not apply to the server due to server configuration. The pass or fail of a test sequence is not affected by omitted test. |
| Skip | Server response doesn't not contain all information that the test requires. Tester should provide additional data to complete the test. A test sequence is treated as failed if skipped tests are not addressed. |
| Fail | Server response is not valid for this test. |
