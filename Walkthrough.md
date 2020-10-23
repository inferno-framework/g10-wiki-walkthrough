# Inferno Walkthrough

This **Walkthrough** introduces Inferno by demonstrating its use as an automated testing tool for the [Draft Test Method](https://www.healthit.gov/topic/certification-ehrs/onc-health-it-certification-program-draft-test-method) of the  [21st Century Cures Act: Interoperability, Information Blocking, and the ONC Health IT Certification Program](https://www.govinfo.gov/content/pkg/FR-2019-03-04/pdf/2019-02224.pdf).  It uses the ONC-hosted Inferno reference instance to test against a publicly available SMART-on-FHIR sandbox.

At the end of this walkthrough, you will be able to use Inferno's draft ONC Program Certification tests to evaluate APIs for conformance to the Proposed ONC certification criteria.  If you are interested in how to use Inferno to test other FHIR-based data exchanges that fall outside the scope of ONC's proposed rule, please visit [Extending Inferno](extending-inferno) in this wiki.

Please note that Inferno is undergoing active development.  Updates to Inferno can be followed on the [Inferno Release Page](https://github.com/onc-healthit/inferno-program/releases).

* [Step 2: Open Inferno](#step-2-open-inferno)
* [Step 3: Select FHIR Version and Enter FHIR Endpoint](#step-3-select-fhir-version-and-enter-fhir-endpoint)
* [Step 4: Perform Discovery and Registration Tests](#step-4-perform-discovery-and-registration-tests)
* [Step 5: Perform Standalone Patient App Tests](#step-5-perform-standalone-patient-app-tests)
* [Step 6: Perform EHR Launch App Tests](#step-6-perform-ehr-launch-app-tests)
* [Step 7: Perform Data Access Tests](#step-7-perform-data-access-tests)
* [Step 8: Review Results](#step-8-review-results)

## Step 1: Open Inferno

* Go to http://inferno.healthit.gov
* Select 'Try It Here' ONC Program Edition', which is an instance of Inferno configured to specifically test the requirements of the criteria in the ONC proposed rule.

![Inferno Splash](images/inferno_landing.png)

## Step 2: Select FHIR Version and Enter FHIR Endpoint

Inferno provides a reference set of tests for FHIR R4. You may use this to test your own fhir end point, or test against the preset Inferno Reference Server.   

* Enter the FHIR URI you would like to test or select the "Or test Inferno against a public sandbox" dropdown and switch it to `Inferno Reference Server`
* Click 'Start Testing'

![Inferno Version Choice](images/inferno_version_choice.png)

## Step 3: Perform Standalone Patient App Tests

Inferno's tests for the ONC certification criteria are organized into four steps.  This allows the tester to walk through the requirements in an order similar to what would be done in a real world situation, while limiting redundant testing.

The first step, 'Standalone Patient App', verifies the ability of a system to perform a Patient Standalone Launch to a SMART on FHIR confidential client with a patient context, refresh token, and OpenID Connect (OIDC) identity token.

![Inferno First Step](images/inferno_first_step.png)

* Click 'Run Tests.'  You will presented with a modal that provides necessary registration information for Inferno. If you opted to test Inferno against a the Inferno reference server, these modal fields will be filled out with the Inferno reference server information.

![Inferno Standalone Patient App - Full Patient Access Modal](images/discover_modal.png)

* Register Inferno as a standalone application for your fhir server.
* In the Inferno Modal, provide the Client ID (and Client Secret if it is a Confidential Client) and click 'Execute'.
* The tests start executing until user input is required. A Tests Running modal will appear to ask if Inferno can redirect to the fhir server's authorization page. Click 'Continue'.
* From here you should follow the fhir server's authorization process.  For the Inferno reference server:
    
* and an overall test result for the entire step will be presented, along with results for each component of the test.  Click on 'details' for more information about any component of the test.

![Discovery Complete](images/discovery_complete.png)

* Inferno provides in-depth information about what occurred during the course of the test to help debug any possible errors.  This includes pass/fail status on any given test, a list of errors, HTTP requests made during the course of the test, and a detailed test description.
* Click 'Show Details' to provide specifics of the test.

![Discovery Complete](images/result_details.png)

* For even more information on any individual test step, click 'results'.
* You have now completed your first test.

