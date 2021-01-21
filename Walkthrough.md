# Inferno Walkthrough

This **Walkthrough** introduces Inferno by demonstrating its use as an automated testing tool for the [Draft Test Method](https://www.healthit.gov/topic/certification-ehrs/onc-health-it-certification-program-draft-test-method) of the  [21st Century Cures Act: Interoperability, Information Blocking, and the ONC Health IT Certification Program](https://www.govinfo.gov/content/pkg/FR-2019-03-04/pdf/2019-02224.pdf).  It uses the ONC-hosted Inferno reference instance to test against a publicly available SMART-on-FHIR sandbox.

At the end of this walkthrough, you will be able to use Inferno's draft ONC Program Certification tests to evaluate APIs for conformance to the Proposed ONC certification criteria.  If you are interested in how to use Inferno to test other FHIR-based data exchanges that fall outside the scope of ONC's proposed rule, please visit [Extending Inferno](extending-inferno) in this wiki.

Please note that Inferno is undergoing active development.  Updates to Inferno can be followed on the [Inferno Release Page](https://github.com/onc-healthit/inferno-program/releases).

* [Step 1: Open Inferno](#step-1-open-inferno)
* [Step 2: Enter FHIR Endpoint](#step-2-enter-fhir-endpoint)
* [Step 3: Perform Standalone Patient App Tests](#step-3-perform-standalone-patient-app-tests)
* [Step 4: Perform Limited App Tests](#step-4-perform-limited-app-tests)
* [Step 5: Perform EHR Practitioner App Tests](#step-5-perform-ehr-practitioner-app-tests)
* [Step 6: Perform Single Patient Api Tests](#step-6-perform-single-patient-api-tests)
* [Step 7: Perform Multi-Patient Api Tests](#step-7-perform-multi-patient-api-tests)
* [Step 8: Perform Additional Tests](#step-8-perform-additional-tests) 

## Step 1: Open Inferno

* Go to https://inferno.healthit.gov
* Select 'Try It Here' under 'ONC Program Edition', which is an instance of Inferno configured to specifically test the requirements of the criteria in the ONC proposed rule.

![Inferno Landing](images/inferno-landing.png)

## Step 2: Enter FHIR Endpoint

Inferno provides a reference set of tests for FHIR R4. You may use this to test your own fhir end point, or test against the preset Inferno Reference Server.   

* Enter the FHIR URI you would like to test or select the "Or test Inferno against a public sandbox" dropdown and switch it to `Inferno Reference Server`.
* Click 'Start Testing'.

![Inferno Endpoint Choice](images/inferno-endpoint-choice.png)

## Step 3: Perform Standalone Patient App Tests

Inferno's tests for the ONC certification criteria are organized into six steps.  This allows the tester to walk through the requirements in an order similar to what would be done in a real world situation, while limiting redundant testing.

The first step, 'Standalone Patient App', demonstrates the ability of a system to perform a Patient Standalone Launch to a SMART on FHIR confidential client with a patient context, refresh token, and OpenID Connect (OIDC) identity token. After launch, a simple Patient resource read is performed on the patient in context. The access token is then refreshed, and the Patient resource is read using the new access token to ensure that the refresh was successful. The authentication information provided by OpenID Connect is decoded and validated, and simple queries are performed to ensure that access is granted to all USCDI data elements.

![Standalone Patient App Tests](images/standalone-patient-app.png)

* Click 'Run Tests.'  You will be presented with a modal that provides necessary registration information for Inferno. If you opted to test Inferno against a the Inferno reference server, these modal fields will be filled out with the Inferno reference server information.

![Inferno Standalone Patient App - Full Patient Access Modal](images/standalone-patient-app-full-patient-access-modal.png)

* Register Inferno as a standalone application for your fhir server.
* In the Inferno Modal, provide the Client ID (and Client Secret if it is a Confidential Client) and click 'Execute'.
* The tests start executing until user input is required. A 'Tests Running' modal will appear to ask if Inferno can redirect to the fhir server's authorization page. Click 'Continue'.

![Tests Running Modal](images/tests-running-redirect-modal.png)

* From here you should follow the fhir server's authorization process.  

* For the Inferno reference server:
  * Select a Patient.

![Patient Select](images/inferno-reference-server-patient-select.png)

* Keep all scopes checked, and click 'Authorize'.

![Scopes Select](images/inferno-reference-server-scopes-select.png)


The authorization process should redirect you back to Inferno, which will continue executing the tests.

You should be able to view the results of the 'Standalone Patient App' tests here. 

![Standalone Patient App Results](images/standalone-patient-app-results.png)

* Inferno provides in-depth information about what occurred during the course of the test to help debug any possible errors.  This includes pass/fail status on any given test, a list of errors, HTTP requests made during the course of the test, and a detailed test description.
* Click 'Show Details' to view specific details about the test.

![Show Details Link](images/standalone-patient-app-show-details-link.png)

![Show Details](images/smart-on-fhir-discovery-view-details.png)

* For even more information on any individual test step, click 'results...'.
* You have now completed your first test.

## Step 4: Perform Limited App Tests

After you have finished reviewing the results from the 'Standalone Patient App' tests, click 'Next' or click on the 'Limited App' tab to progress to the next step in the test procedure. This scenario demonstrates the ability to perform a Patient Standalone Launch to a SMART on FHIR confidential client with limited access granted to the app based on user input. The tester is expected to grant the application access to a subset of desired resource types, and to deny requests for “offline_access” refresh tokens. 

* Click on the 'Run Tests' button to begin.

![Limited App](images/limited-app.png)

* Note the resources listed in the Expected Resource Grant.

![Limited App Modal](images/limited-app-modal.png)

*  Once you click 'Execute', similar to the Standalone Patient App Tests, Inferno will notify you that it is redirecting you to the Authorization server as part of the SMART on FHIR / OAuth launch sequence. Click 'Continue' to redirect to the FHIR server's authorization process.

![Limited App Redirect](images/limited-app-redirect.png)

* From here you should follow the FHIR server's authorization process.  

For the Inferno reference server:

* Select a Patient.

![Patient Select](images/inferno-reference-server-patient-select.png)

* Deselect all scopes except for `patient/Condition.read`, `patient/Observation.read`, and `patient/Patient.read` (resources listed in the Expected Resource Grant in the previous step), and click 'Authorize'.

![Limited App Scope Select](images/limited-app-scopes-select.png)

The authorization process should redirect you back to Inferno, which will continue executing the tests.

![Limited App Results](images/limited-app-results.png)

## Step 5: Perform EHR Practitioner App Tests

Continue on to the 'EHR Practitioner App' set of tests.  This set of tests requires the user to initiate an app launch outside of Inferno in order to fully demonstrate the ability of the server to support the EHR Launch flow as described in the SMART App Launch Guide.  Inferno tests this by pausing this set of tests mid-execution, and waits at the specified launch point for the user to initiate the launch sequence from the EHR.  This action will then inform Inferno that the test may continue running, with information provided during the launch.

* Click Run Tests. The EHR Practitioner App modal should appear.

![EHR Practitioner App](images/ehr-practitioner-app.png)

* Provide Client ID (and Client Secret if Confidential).
* Click 'Execute' to begin the tests.

![EHR Practitioner App Modal](images/ehr-practitioner-app-modal.png)

* The tests will begin executing and immediately the interface will notify the user that Inferno needs to receive an external action in order to continue. In this case, Inferno is waiting for the user to initiate and app launch from the EHR.

![EHR Waiting](images/ehr-practitioner-app-waiting.png)

* Launch the app from your EHR from the provided app.  

For the Inferno reference server:
* Go to https://inferno.healthit.gov/reference-server/app/app-launch.
* Enter in the provided launch uri (https://inferno.healthit.gov/inferno/oauth2/static/launch).
* Click Launch App.

![EHR Launch](images/inferno-reference-server-app-launch.png)

* From this point on, the tests will execute in a similar manner to the 'Standalone Launch' sequence provided earlier.

* And finally, results will be displayed in a similar manner to the previous test groups.

![EHR Practitioner App Results](images/ehr-practitioner-app-results.png)

## Step 6: Perform Single Patient API Tests

At this point, the user should have received a Patient ID and be authorized to perform the required FHIR queries on the FHIR server.  Click 'Next' or on the 'Single Patient Api' tab to begin testing that capability.

![Single Patient Api](images/single-patient-api.png)

* Click 'Run Tests'.

* The user will be shown the Bearer Token collected earlier, as well as the Patient ID returned *on the most recent SMART Launch*.  This may have been either the Standalone Launch or Patient launch -- this set of tests currently does not require users to demonstrate all of these queries in both situations.

![Single Patient Api Modal](images/single-patient-api-modal.png)

* Click 'Execute'.
* After running these tests, you will be presented with the test results.  These tests typically follow this pattern:
  * Ensure that the user does not have access to searching without the appropriate authorization header.
  * Perform a FHIR search for all resources of a certain type that are associated with the relevant patient.
  * For each of the filtered searches required by US Core / Argonaut, generate search queries that should return at least one result based on data that has already been seen, and verify that all data returned falls within the search criteria.
  * Validate all resources returned against the relevant profile.  This includes validating that codes are within required ValueSets.
  * Ensure that all references contained within the resource can be retrieved.
* Note: if the selected patient does not include all required resources, then some tests will be marked as 'SKIP'.  The tester can then execute one of the Launch Sequence tests and authorize another patient, and only execute the tests that were previously skipped.  This allows the test system to have the flexibility to demonstrate that all data can be returned, without requiring a single patient to have all required data elements.

## Step 7: Perform Multi-Patient API Tests

The Multi-Patient Authorization and API tests demonstrate the ability to export clinical data for multiple patients in a group using FHIR Bulk Data Access IG. This test uses Backend Services Authorization to obtain an access token from the server. After authorization, a group level bulk data export request is initialized. Finally, this test reads exported NDJSON files from the server and validates the resources in each file. To run the test successfully, the selected group export is required to have every type of resource mapped to USCDI data elements. Additionally, it is expected the server will provide Encounter, Location, Organization, and Practitioner resources as they are referenced as must support elements in required resources. 

![Multi Patient Api](images/multi-patient-api.png)

* Click 'Run Tests'. The Multi-Patient Authorization and API Modal will appear.

![Multi Patient Api Modal](images/multi-patient-api-modal.png)

 * Fill Bulk Data FHIR URL, Backend Services Token Endpoint, Bulk Data Client ID, Bulk Data Scopes, and Group ID.
 * Click 'Execute'.

## Step 8: Perform Additional Tests

Not all requirements that need to be tested fit within the previous scenarios. The tests contained in this section addresses remaining testing requirements. Each of these tests need to be run independently. Please read the instructions for each in the ‘About’ section, as they may require special setup on the part of the tester.

In this test, each section is run separately.

* Public Client Standalone Launch With OpenId Connect
Register Inferno as a public client with patient access and execute standalone launch.
  * Click 'Run'.
  * Fill out the Public Client Standalone Launch with OpenID Connect Modal.
  * Click 'Execute'.
  * Follow the Redirect Authorization process similar to the Standalone Patient App Tests.

* Token Revocation
This test demonstrates the Health IT module is capable of revoking access granted to an application. This test relies on the user to verify that token was revoked.

  * Click on 'Run Tests'.

  * Revoke a Token through the EHR.  For the Inferno Reference Server:

    * Click 'Run' and fill out the Token Revocation modal with the correct FHIR Endpoint, OAuth 2.0 Token Endpoint, and the Revoked Bearer Token and Corresponding Refresh Token.

![Multi Patient Api Modal](images/token-revocation-modal.png)

    * Click 'Execute'.
    * Go to https://inferno.healthit.gov/reference-server/oauth/token/revoke-token in another tab and insert the copied Token value from the modal into the text input and click 'Revoke'. This will also revoke the corresponding refresh token.

![Inferno Reference Server Revoke Token](images/inferno-reference-server-revoke-token.png)


* SMART App Launch Error: Invalid Aud Parameter
The purpose of this test is to demonstrate that the server properly validates AUD parameter
  * Click 'Run'.
  * Fill out the SMART App Launch Error: Invalid AUD Parameter modal.

![Smart App Launch Error Invalid Aud Tests Running Modal](images/smart-app-authorization-error-invalid-aud-parameter-modal.png)

  * Click 'Execute'. The Test Running modal will appear.

![Smart App Launch Error Invalid Aud Tests Running Modal](images/smart-app-launch-error-invalid-aud-tests-running-modal.png)

  * Click 'Perform Invalid Launch in New Window' which should open a new tab that redirects you to the fhir server's authorization process.  The purpose of this test is to confirm that the fhir server does NOT return back to Inferno, but instead displays an error message indicating that the aud value is invalid. 

  * For example, with the Inferno reference server:

![Smart App Launch Error Invalid Aud Tests Running Modal](images/inferno-reference-server-invalid-aud.png)

  * As soon as you have confirmed that the redirect displays an error, go back to your Inferno tab click 'Attest Launch Failed'.

* SMART App Launch Error: Invalid Launch Parameter
The purpose of this test is to demonstrate that the server properly validates LAUNCH parameter
  * Click 'Run'.
  * Fill out the SMART App Launch Error: Invalid Launch Parameter modal.

![Smart App Launch Parameters Invalid Launch Tests Running Modal](images/smart-app-launch-parameters-invalid-launch-param-modal.png)

  * Click 'Execute'.

  * The Waiting at LAUNCH URI modal will appear. Launch from your EHR with the provided LAUNCH URI. Similar to the EHR Practitioner App Tests.

  * Launch the app from your EHR from the provided app.  For the Inferno reference server:
    * Go to https://inferno.healthit.gov/reference-server/app/app-launch
    * Enter in the provided launch uri (https://inferno.healthit.gov/inferno/oauth2/static/launch)
    * Click 'Launch App'. 
      * Launching from the EHR should redirect you to Inferno with the 'Tests Running' modal open.

![Smart App Launch Parameters Invalid Launch Tests Running Modal](images/smart-app-launch-error-invalid-aud-tests-running-modal.png)

  * Click 'Perform Invalid Launch in New Window'. This should open a new tab in your EHR where you should receive an error message stating that the Launch is invalid. For example, with the Inferno Reference Server:

![Inferno Reference Server Launch Is Invalid](images/inferno-reference-server-launch-is-invalid.png)

  * Go back to your Inferno tab, and click 'Attest Launch Failed'. 

* Visual Inspection And Attestation
The purpose of this test is to verify conformance to portions of the test procedure that are not automated.
  * Click 'Run'.
  * The 'Visual Inspection and Attestation modal will appear, with a list of Yes/No radio buttons and text boxes for Notes.  Fill out this form, and click 'Execute'.

![Visual Inspection and Attestation Modal](images/visual-inspection-and-attestation-modal.png)







