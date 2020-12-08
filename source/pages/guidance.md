---
title: General Guidance
layout: default
active: terminology
---

This section outlines important definitions and interpretations and requirements common to all AEGIS Touchstone Testing actors used in this guide.
The conformance verbs used are defined in [FHIR Conformance Rules].

---

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->
**Contents**

* Do not remove this line (it will not be displayed)
{:toc}

---

<!-- end TOC -->

## Rule and RuleSet Extensions

The TestScript rule and ruleset extensions can be used to reference complex validation logic that goes beyond what the basic TestScript assert element supports. As such, rules are recommended only when a TestScript assert cannot be used in its basic form.

The Touchstone Rules-Engine supports rules written in the following languages:

- Groovy
- XSLT
- Schematron

Support for additional languages may be added in the future. Unless you plan on executing test scripts against a test system that only supports XML, it is highly recommended to write rules in Groovy as XSLT and Schematron rules can only be evaluated against requests and responses whose content is in XML while Groovy supports JSON as well.

------------------------------------------------------------------------

## OAuth2 Capabilities

Touchstone has the ability for a user to create and use a Test System that is OAuth2 enabled, and with that comes new features that are important to take note of when it comes to Test Executions performed against a Test System that has an OAuth2 Authorization service connected to it.

### TestScript Extensions

With the addition of a more robust OAuth2, SMART-on-FHIR and Bulk Data support into Touchstone, there are new extensions under the TestScript.test and TestScript.test.operation elements to support the manual control of test execution behavior and execution of the various OAuth2 authorization flows that are available to test script authors.

* [AEGIS Touchstone Testing TestScript Operation AuthorizeInNewTab Extension](StructureDefinition-testscript-operation-authorizeInNewTab.html)
* [AEGIS Touchstone Testing TestScript Operation Oauth2AuthzRedirectId Extension](StructureDefinition-testscript-operation-oauth2AuthzRedirectId.html)
* [AEGIS Touchstone Testing TestScript Operation Oauth2AuthzRequestId Extension](StructureDefinition-testscript-operation-oauth2AuthzRequestId.html)
* [AEGIS Touchstone Testing TestScript Operation SmartLaunchRequestId Extension](StructureDefinition-testscript-operation-smartLaunchRequestId.html)
* [AEGIS Touchstone Testing TestScript Test ManualCompletion Extension](StructureDefinition-testscript-test-manualCompletion.html)

### Pre-defined Fixture Ids

With the addition of a more robust OAuth2 environment into Touchstone, there are new fixtures, and values under those fixtures, that are available to test script authors.

* One new fixture is the _dest1SmartConfig_ (_dest2SmartConfig_, _dest3SmartConfig_, etc.). The fixture allows the test script author to access different variables coming from the JSON document that is at the _.well-known/smart-configuration.json_ endpoint. It acts the same as retrieving variables from the capabilities statement. For example, to use _dest1SmartConfig_ to retrieve the Client ID, you would use the <span style="color:red">sourceId</span> and <span style="color:red">path</span> in the testscript as <span style="color:red">sourceId</span> = dest1SmartConfig, <span style="color:red">path</span> = $.clientId.

* Test Systems have special Oauth2 values that can be be retrieved by test script authors. The fixtures have a naming convention of _dest1SystemConfig_, _dest2SystemConfig_, _origin1SystemConfig_, _origin2SystemConfig_, etc. These fixtures have the following attributes:

<table border="1" cellspacing="0" cellpadding="5" style="border-collapse:collapse;border:solid windowtext 1pt">
	<thead>
		<tr>
			<td style="font-weight:bold">Attribute Name</td>
			<td style="font-weight:bold">Description</td>
		</tr>
	</thead>
	<tr>
		<td>name</td>
		<td>The Test System name</td>
	</tr>
	<tr>
		<td>fullName</td>
		<td>The Organization name + the Test System name</td>
	</tr>
	<tr>
		<td>baseUrl</td>
		<td>The Base URL of the Test System</td>
	</tr>
	<tr>
		<td>supportsSmartOnFhir</td>
		<td>Set to <span style="color:red">true</span> if the Test System supports SMART on FHIR, <span style="color:red">false</span> if it does not</td>
	</tr>
	<tr>
		<td>oauth2GrantType</td>
		<td>The Grant Type of the OAuth2 enables Test System, either Authorization Code or Client Credentials</td>
	</tr>
	<tr>
		<td>clientId</td>
		<td>The Client ID of the Test System that is registered with the OAuth2 server.</td>
	</tr>
	<tr>
		<td>clientSecret</td>
		<td>The Client Secret of the Test System that is registered with the OAuth2 server.</td>
	</tr>
	<tr>
		<td>authEndpoint</td>
		<td>The Authorization Endpoint for the Test System.</td>
	</tr>
	<tr>
		<td>tokenEndpoint</td>
		<td>The Token Endpoint for the Test System.</td>
	</tr>
	<tr>
		<td>registerEndpoint</td>
		<td>The Registration Endpoint for the Test System.</td>
	</tr>
	<tr>
		<td>introspectEndpoint</td>
		<td>The Introspection Endpoint for the Test System.</td>
	</tr>
	<tr>
		<td>revocationEndpoint</td>
		<td>The Revocation Endpoint for the Test System.</td>
	</tr>
	<tr>
		<td>scopesSupported</td>
		<td>The scopes that allowed for the server access to certain user scopes</td>
	</tr>
</table>

* Another set of fixtures that can be used by the user in the test scripts are oauth2AuthzRequest and oauth2AuthzRedirect. These two fixtures are used to access parts the the last OAuth2 Authroization Request sent to the OAuth2 server and the last OAuth2 Authorization response returned from the server.

### Base64Encoding

Touchstone is configured to use a *Base64Encoding* on the operation.requestHeader.value when it includes <span style="color:red">Basic</span> + <span style="color:red">A Single Space " "</span> ahead of the value and when the operation.requestHeader.name is equal to <span style="color:red">Authorization</span> in your Testscript executions. This is done for security and is the explanation as to why an <span style="color:red">Authorization</span> value in the header will be different from the one that was originally coded.

------------------------------------------------------------------------

## Bulk Data Capabilities

Touchstone has the ability for a user to create and use a Test System that supports the Bulk Data specification. Touchstone provides additional features that provide support for the evaluation and validation of the NDJSON returned files.

### NDJSON File Evalutation and Validation

The [Bulk Data specification](https://hl7.org/fhir/uv/bulkdata/index.html) defines a new FHIR extended operation [$export](https://hl7.org/fhir/uv/bulkdata/export/index.html) which generates bulk data output files containing multiple FHIR resources using the [NDJSON](http://ndjson.org) format for FHIR - [application/fhir+ndjson](https://www.hl7.org/fhir/nd-json.html).

Evaluation and validation of this NDJSON format for FHIR requires extended capabilities be implemented on the FHIR Test Engine and extended definitions within the FHIR TestScript resource. Touchstone has implemented these extended capabilities through the use of **NDJSON Assertion Prefix** syntax within the values of the following TestScript elements:

* TestScript.profile.reference
* TestScript.test.action.assert.expression
* TestScript.test.action.assert.path
* TestScript.test.action.assert.resource

**NDJSON Assertion Prefix** syntax is specified within curly braces proceeding the normal expected content of these element values and is composed of 3 optional parts:

<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator</span> | <span style="color:purple">Filter-Index-Range</span> | <span style="color:red">Filter-Path</span> } Regular-Assert</span>

The **NDJSON Assertion Prefix** syntax is applied in 4 stages:

<ol>
<li><span style="font-weight:bold; color:red">Filter-Path</span> evaluation. Example: <span style="font-weight:bold; color:red">.name[?(@.family=='Gracia')]</span> - all Patients whose name.family is 'Gracia'. A resource is included if the JSON-Path evaluation results in a <span style="font-weight:bold">Truthy</span> value (exists and is not false). If no <span style="color:red">Filter-Path</span> is specified then all resources filter through.</li>
<li><span style="font-weight:bold; color:purple">Filter-Index-Range</span>. Example: <span style="font-weight:bold; color:purple">1-10</span> - include resources indexed 1 through 10 in the assertion evaluation. If <span style="color:purple">Filter-Index-Range</span> is <span style="font-weight:bold">not</span> specified then all resources filter through. If <span style="color:red">Filter-Path</span> is specified then <span style="color:purple">Filter-Index-Range</span> operates on the resources that made it through the <span style="color:red">Filter-Path</span> and <span style="font-weight:bold">not</span> the original resources.</li>
<li><span style="font-weight:bold">Regular-Assert</span>. The Profile/Expression/Path/Resource evaluation on resources that made it past 1 and 2. Example: <span style="font-weight:bold">Patient</span> (if assertion is resource).</li>
<li><span style="font-weight:bold; color:green">Evaluation-Operator</span>. '<span style="font-weight:bold; color:green">any</span>' or '<span style="font-weight:bold; color:green">all</span>' - If '<span style="color:green">any</span>' operator is used then overall assertion evaluation passes if a single resource passes. The default is '<span style="color:green">all</span>'.</li>
</ol>

### NDJSON Assertion Prefix

#### TestScript Example

The following TestScript provides a comprehensive list of asserts that show various usage examples of the **NDJSON Assertion Prefix** Syntax.

* [NDJSON Assertion Prefix Syntax Example](TestScript-ndjson-assertion-prefix.html)

<p id="publish-box">Here are some portions from this TestScript illustrating key examples of the **NDJSON Assertion Prefix** syntax.</p>

#### TestScript.profile.reference

Using the **NDJSON Assertion Prefix** syntax in the TestScript.profile.reference element allows for specific profile definitions with this syntax to be used in any assert.validateProfileId evaluation; i.e. where the FHIR Validation Engine is invoked. The syntax stages are applied in the same sequence where the FHIR Validation Engine invocation is executed a step <span style="font-weight:bold">3 Regular-Assert</span>.

Here are three (3) profile definitions and corresponding asserts showing the use of each optional **NDJSON Assertion Prefix** syntax item.

<ol>
<li>
<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator 'any'</span> } Regular-Assert</span>
<pre><code>&lt;profile id="resource-profile-for-any"&gt;
  &lt;!-- Assert passes as long as any resource in the NDJSON contents validates successfully against the FHIR Patient profile. --&gt;
  &lt;reference value="{any}http://hl7.org/fhir/StructureDefinition/Patient"/&gt;
&lt;/profile&gt;
...
&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Validate that the returned resource conforms to the corresponding FHIR resource profile."/&gt;
    &lt;direction value="response"/&gt;
    &lt;validateProfileId value="resource-profile-for-any"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:purple">Filter-Index-Range</span> } Regular-Assert</span>
<pre><code>&lt;profile id="resource-profile-for-first"&gt;
  &lt;!-- Assert passes as long as the first five (5) resources in the NDJSON contents validates successfully against the FHIR Patient profile. --&gt;
  &lt;reference value="{1-5}http://hl7.org/fhir/StructureDefinition/Patient"/&gt;
&lt;/profile&gt;
...
&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Validate that the returned resource conforms to the corresponding FHIR resource profile."/&gt;
    &lt;direction value="response"/&gt;
    &lt;validateProfileId value="resource-profile-for-first"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:red">Filter-Path</span> } Regular-Assert</span>
<pre><code>&lt;profile id="resource-profile-for-Gracia"&gt;
  &lt;!-- Assert passes as long as the those resources where name.family equals 'Garcia' in the NDJSON contents validates successfully against the FHIR Patient profile. --&gt;
  &lt;reference value="{.name[?(@.family=='Gracia')]}http://hl7.org/fhir/StructureDefinition/Patient"/&gt;
&lt;/profile&gt;
...
&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Validate that the returned resource conforms to the corresponding FHIR resource profile."/&gt;
    &lt;direction value="response"/&gt;
    &lt;validateProfileId value="resource-profile-for-Gracia"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
</ol>

#### TestScript.test.action.assert.expression

The following asserts show the use of various combinations of the **NDJSON Assertion Prefix** syntax items within the assert.expression element. Please refer to each assert.description element for details of expected behavior.

<ol>
<li>
<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator 'any'</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as any resource in the NDJSON contents contains a Patient.name.family value in 'Allen,Gracia'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;expression value="{any}Patient.name.family"/&gt;
    &lt;operator value="in"/&gt;
    &lt;value value="Allen,Gracia"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator 'all'</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as all resource in the NDJSON contents contain a Patient.name.family value equal to 'Allen'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;expression value="{all}Patient.name.family"/&gt;
    &lt;operator value="equals"/&gt;
    &lt;value value="Allen"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:purple">Filter-Index-Range</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as the first three (3) resources in the NDJSON contents contain a Patient.name.family value in 'McKay,Gracia,Allen'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;expression value="{1-3}Patient.name.family"/&gt;
    &lt;operator value="in"/&gt;
    &lt;value value="McKay,Gracia,Allen"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:red">Filter-Path</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as the resources in the NDJSON contents that contain a name.family value equal to 'Allen' contain a Patient.name.given value in 'Carol,G'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;expression value="{.name[?(@.family=='Allen')]}Patient.name.given"/&gt;
    &lt;operator value="in"/&gt;
    &lt;value value="Carol,G"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:purple">Filter-Index-Range</span> | <span style="color:red">Filter-Path</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as the first three (3) resources in the NDJSON contents that contain a name.family value equal to 'McKay' contain a Patient.name.given value equal to 'George'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;expression value="{1-3 | .name[?(@.family=='McKay')]}Patient.name.given"/&gt;
    &lt;operator value="equals"/&gt;
    &lt;value value="George"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator 'any'</span> | <span style="color:purple">Filter-Index-Range</span> | <span style="color:red">Filter-Path</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as the any of the first three (3) resources in the NDJSON contents that contain a name.family value equal to 'McKay' contain a Patient.name.given value equal to 'George'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;expression value="{any | 1-3 | .name[?(@.family=='McKay')]}Patient.name.given"/&gt;
    &lt;operator value="equals"/&gt;
    &lt;value value="George"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
</ol>

#### TestScript.test.action.assert.path

The following asserts show the use of various combinations of the **NDJSON Assertion Prefix** syntax items within the assert.path element. Please refer to each assert.description element for details of expected behavior.

<ol>
<li>
<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator 'any'</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as any resource in the NDJSON contents contains a generalPractitioner.reference value"/&gt;
    &lt;direction value="response"/&gt;
    &lt;operator value="equals"/&gt;
    &lt;path value="{any}generalPractitioner.reference"/&gt;
    &lt;value value="Practitioner/2"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:purple">Filter-Index-Range</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as the 2-5 resources in the NDJSON contents contain a generalPractitioner.reference value that contains 'Practitioner'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;operator value="contains"/&gt;
    &lt;path value="{2-5}generalPractitioner.reference"/&gt;
    &lt;value value="Practitioner/"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:red">Filter-Path</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as the resources in the NDJSON contents that contain a generalPractitioner.reference value equal to 'Practitioner/3' contain a name.family value equal to 'Allen'"/&gt;
    &lt;direction value="response"/&gt;
    &lt;operator value="equals"/&gt;
    &lt;path value="{generalPractitioner[?(@.reference=='Practitioner/3')]}name.family"/&gt;
    &lt;value value="Allen"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
</ol>

#### TestScript.test.action.assert.resouce

The following asserts show the use of various combinations of the **NDJSON Assertion Prefix** syntax items within the assert.resource element. Please refer to each assert.description element for details of expected behavior.

<span style="color:red"><span style="font-weight:bold">WARNING</span> - THE USE OF THE **NDJSON Assertion Prefix** SYNTAX WITHIN THE **assert.resource** ELEMENT PREVENTS THE CONTAINING TESTSCRIPT RESOURCE FROM VALIDATING ON UPLOAD TO TOUCHSTONE.</span>
<ol>
<li>
<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator 'any'</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as any resource in the NDJSON contents is a FHIR Patient resource type"/&gt;
    &lt;direction value="response"/&gt;
    &lt;resource value="{any}Patient"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:purple">Filter-Index-Range</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as the first five (5) resources in the NDJSON contents is a FHIR Patient resource type"/&gt;
    &lt;direction value="response"/&gt;
    &lt;resource value="{1-5}Patient"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
<li>
<span style="font-weight:bold">{ <span style="color:red">Filter-Path</span> } Regular-Assert</span>
<pre><code>&lt;action&gt;
  &lt;assert&gt;
    &lt;extension url="http://touchstone.aegis.net/touchstone/fhir/testing/StructureDefinition/testscript-assert-stopTestOnFail"&gt;
      &lt;valueBoolean value="false"/&gt;
    &lt;/extension&gt;
    &lt;description value="Assert passes as long as all the resources in the NDJSON contents that contain a name.family value equal to 'Gracia' is a FHIR Patient resource type"/&gt;
    &lt;direction value="response"/&gt;
    &lt;resource value="{.name[?(@.family=='Gracia')]}Patient"/&gt;
    &lt;warningOnly value="false"/&gt;
  &lt;/assert&gt;
&lt;/action&gt;</code></pre>
</li>
</ol>

------------------------------------------------------------------------

{% include link-list.md %}
