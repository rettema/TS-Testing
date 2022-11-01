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

## Touchstone Placeholders

The Touchstone Placeholders are special predefined TestScript variables and may be used where standard TestScript variables are allowed: static fixtures and operation.params, operation.requestHeader.value, operation.url, and assert.value element values.

These Touchstone Placeholders fall into two (2) categories

- Predefined User Unique Data Values
- Functions for Dynamic Data Generation

### Predefined User Unique Data Values

The predefined user unique data value placeholders are strings of varying lengths from 1 to 20 characters. They are defined of three (3) types:

- Character only; for example, <span style="font-weight:bold; color:blue">${C6}</span>
- Digits only; for example, <span style="font-weight:bold; color:blue">${D9}</span>
- Characters + Digits; for example, <span style="font-weight:bold; color:blue">${CD14}</span>

Users may view their assigned placeholder variable values in Touchstone via the user name menu item <span style="font-weight:bold; color:blue">${} My Placeholders</span>.

<span style="color:red"><span style="font-weight:bold">Guaranteed Value Uniqueness</span> - All predefined user unique data value placeholder values with a character length of 6 or more are guaranteed to be unique.</span>

### Functions for Dynamic Data Generation

There are three (3) available functional constructs for dynamic data generation:

- CURRENTDATE and CURRENTDATETIME
- DATE and DATETIME (relative to dynamic variable)
- UUID, UUID-ST, UUID-NODASH and UUID-ST-NODASH

#### CURRENTDATE[TIME]

These function placeholder variables provide support for date and datetime values based on the current date (today) and current datetime (now). Optional comma separated offset arguments apply date and time arithmetic providing relative date and time generated values.

**Syntax/Format** ${&lt;placeholder name&gt;[, &lt;datetime portion code&gt;, &lt;offset value&gt;]}
Where,
* &lt;placeholder name&gt; is **CURRENTDATE** or **CURRENTDATETIME**
* &lt;datetime portion code&gt; is the character indicating what portion of the date or datetime value will be offset. Valid characters are derived from the following Java SimpleDateFormat pattern string "yyMMddHHmmss"
* &lt;offset value&gt; is the signed integer value used to adjust the date or time
* &lt;datetime portion code&gt;, &lt;offset value&gt; may be repeated for more complex arithmetic

**Examples**
<table border="1" cellspacing="0" cellpadding="5" style="border-collapse:collapse;border:solid windowtext 1pt">
	<thead>
		<tr>
			<td style="font-weight:bold">Placeholder Example</td>
			<td style="font-weight:bold">Description</td>
		</tr>
	</thead>
	<tr>
		<td>${CURRENTDATE}</td>
		<td>The current date (today)</td>
	</tr>
	<tr>
		<td>${CURRENTDATETIME}</td>
		<td>The current date and time (today)</td>
	</tr>
	<tr>
		<td>${CURRENTDATE,d,-10}</td>
		<td>The date 10 days before the current date</td>
	</tr>
	<tr>
		<td>${CURRENTDATETIME,d,10}</td>
		<td>The date and time 10 days after the current date and time</td>
	</tr>
	<tr>
		<td>${CURRENTDATE,y,-1}</td>
		<td>The date 1 year before the current date</td></tr>
	<tr>
		<td>${CURRENTDATETIME,H,10}</td>
		<td>The date and time 10 hours after the current date and time</td>
	</tr>
</table>

#### DATE[TIME]

These function placeholder variables provide support for date and datetime values based on a dynamic variable user input date or datetime value. Optional comma separated offset arguments apply date and time arithmetic providing relative date and time generated values.

**Syntax/Format** ${&lt;placeholder name&gt;, &lt;relative value&gt;[, &lt;datetime portion code&gt;, &lt;offset value&gt;]}
Where,
* &lt;placeholder name&gt; is **DATE** or **DATETIME**
* &lt;relative value&gt; is a dynamic variable defined in the current TestScript that holds the relative date or datetime value
  * <span style="color:red">The dynamic variable must be defined in TestScript without path/expression; its value will be entered by the user during Test Setup</span>
* &lt;datetime portion code&gt; is the character indicating what portion of the date or datetime value will be offset. Valid characters are derived from the following Java SimpleDateFormat pattern string "yyMMddHHmmss"
* &lt;offset value&gt; is the signed integer value used to adjust the date or time
* &lt;datetime portion code&gt;, &lt;offset value&gt; may be repeated for more complex arithmetic

**Examples**
<table border="1" cellspacing="0" cellpadding="5" style="border-collapse:collapse;border:solid windowtext 1pt">
	<thead>
		<tr>
			<td style="font-weight:bold">Placeholder Example</td>
			<td style="font-weight:bold">Description</td>
		</tr>
	</thead>
	<tr>
		<td>${DATE, medicationDate}</td>
		<td>The value of the user entered medicationDate will be used as-is</td>
	</tr>
	<tr>
		<td>${DATETIME, medicationDateTime}</td>
		<td>The value of the user entered medicationDateTime will be used as-is</td>
	</tr>
	<tr>
		<td>${DATE, medicationDate, d, -10}</td>
		<td>The value of the user entered medicationDate minus 10 days will be used</td>
	</tr>
	<tr>
		<td>${DATETIME, medicationDateTime, M, -1}</td>
		<td>The value of the user entered medicationDateTime minus 1 month will be used</td>
	</tr>
</table>

#### UUID[-ST|-NODASH|-ST-NODASH]

These function placeholder variables provide support for generation of UUID values with predefined formatting.

**Syntax/Format** ${&lt;placeholder name&gt;}
Where,
* &lt;placeholder name&gt; is **UUID**, **UUID-ST**, **UUID-NODASH** or **UUID-ST-NODASH**

**Examples**
<table border="1" cellspacing="0" cellpadding="5" style="border-collapse:collapse;border:solid windowtext 1pt">
	<thead>
		<tr>
			<td style="font-weight:bold">Placeholder Example</td>
			<td style="font-weight:bold">Description</td>
		</tr>
	</thead>
	<tr>
		<td>${UUID}</td>
		<td>A single generated UUID string value without the standard prefix; e.g., '3ed6eb79-fc68-443a-996f-08167f5bdef0'</td>
	</tr>
	<tr>
		<td>${UUID-ST}</td>
		<td>A single generated UUID string value including the standard prefix; e.g., 'urn:uuid:3ed6eb79-fc68-443a-996f-08167f5bdef0'</td>
	</tr>
	<tr>
		<td>${UUID-NODASH}</td>
		<td>A single generated UUID string value without the standard prefix and dash characters removed; e.g., '3ed6eb79fc68443a996f08167f5bdef0'</td>
	</tr>
	<tr>
		<td>${UUID-ST-NODASH}</td>
		<td>A single generated UUID string value including the standard prefix and dash characters removed; e.g., 'urn:uuid:3ed6eb79fc68443a996f08167f5bdef0'</td>
	</tr>
</table>

### Usage

The Touchstone Placeholder, both predefined values and functions, are typically defined in static fixtures used as an operation request payload; for example, create or update. Touchstone's TestScript Execution interface provides both a Raw and Resolved view of static fixtures where the pre and post test execution contents can be examined.

**Raw Example** - _Patient.name_, _Patient.birthDate_
<pre><code>&lt;name&gt;
  &lt;use value="official"/&gt;
  &lt;family value="Smith${C7}"/&gt;
  &lt;given value="John${C6}"/&gt;
  &lt;birthDate value="${CURRENTDATE,d,-7}"/&gt;
&lt;/name&gt;</code></pre>

**Resolved Example** - _Patient.name_, _Patient.birthDate_
<pre><code>&lt;name&gt;
  &lt;use value="official"/&gt;
  &lt;family value="SmithMqCERSQ"/&gt;
  &lt;given value="JohnXRCnsc"/&gt;
  &lt;birthDate value="2021-01-27"/&gt;
&lt;/name&gt;</code></pre>

------------------------------------------------------------------------

## Minimum ID (minimumID) Assert Processing

The FHIR specification's definition of [FHIR Testing](http://hl7.org/fhir/testing.html) contains a brief description within the [Test search operation](http://hl7.org/fhir/testing.html#search) section on how a Test Engine is to evaluate and process a TestScript assert using the [minimumId element](http://hl7.org/fhir/testscript-definitions.html#TestScript.setup.action.assert.minimumId). This description states:

~~~
<action>
	<assert>
		<minimumId value="F1"/>
		<sourceId value="R1"/>
	</assert>
</action>

Test engines will parse the 'body' of the F1 fixture and verify that each element and its value matches the corresponding element in the R1 response body. In other words, R1 is verified to be a 'superset' of F1. The resource id element in the body will be ignored during comparison. The headers will also be ignored.

F1 can be statically defined or it can be the [responseId](http://hl7.org/fhir/testscript-definitions.html#TestScript.setup.action.operation.responseId) for another operation. If [sourceId](http://hl7.org/fhir/testscript-definitions.html#TestScript.setup.action.assert.sourceId) is not specified, then test engines will use the response of the last operation.
~~~

This is a high-level functional description that does not provide sufficient information regarding the required definition of how the detailed comparison is to be performed.  In order to provide more clarity the Touchstone implementation of the minimumId assert comparison logic now provides the following structural comparison of the minimumId fixture and the fixture being evaluated behaviors:

* **Attribute or element ordering** - If the comparison encounters an order mismatch of an attribute or element between the minimumId fixture and the fixture being evaluated, this does not result in an error as long as the path to the attribute or element matches.
  * This is particularly important when comparing JSON formatted fixtures where attribute order is not significant.
  * For example, the following two fixtures are considered to be matching:
    ~~~
    {
      name: "hello",
      title: "goodbye"
    }
    
    {
      title: "goodbye",
      name: "hello"
    }
    ~~~

* **Array value ordering** - The order of an array of the same attribute or element is allowed to differ in the comparison; also, additional attribute or element instances are allowed in between the minimumId fixture attribute or element instances such that the index can differ.
  * For example, the following two fixtures are considered equal even though the order of array elements differs:
    ~~~
    min fixture:
    {
      names: ["hello", "goodbye"]
    }
    
    compare fixture:
    {
      names: ["goodbye", "hello"]
    }
    ~~~
  * For example, these fixtures would match even though there are extra values:
    ~~~
    min fixture:
    {
      names: ["hello", "goodbye"]
    }
    
    compare1 fixture:
    {
      names: ["hello", "greeting", "goodbye"]
    }
    
    compare2 fixture:
    {
      names: ["greeting", "hello", "goodbye"]
    }
    
    compare3 fixture:
    {
      names: ["hello", "goodbye", "greeting"]
    }
    ~~~

* **Duplicate attributes or elements** - When the minimumId fixture has duplicate attributes or elements (in a JSON attribute array or XML repeated element) the fixture being evaluated must have the same number of duplicated attributes or elements.
  * For example, the following two fixtures would not match as the compare fixture does not have the same number of duplicate repeating attributes as the min fixture:
    ~~~
    min fixture:
    {
      names: ["hello", "hello"]
    }
    
    compare1 fixture:
    {
      names: ["hello"]
    }
    ~~~

* **Comparison results** - The comparison results now include ALL inconsistencies between the minimumId fixture and the fixture being evaluated (previously only the first mismatch was reported back to the user).

* **Attribute or element present regardless of value** - The ability to allow for the existence of an attribute or element regardless of its value is possible with the XML syntax but not JSON due to the JSON requirement that an attribute value must be present.
  * For example, this minimumId fixture XML element without a value will match any same named XML element in the fixture being evaluated regardless of its value:
    ~~~
    <novalueelement/>
    ~~~
  * This concept cannot be expressed in JSON syntax.

------------------------------------------------------------------------

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
* [AEGIS Touchstone Testing TestScript Operation PagesNext Extension](StructureDefinition-testscript-operation-pagesNext.html)
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

### NDJSON Assertion Prefix Syntax and Usage

**NDJSON Assertion Prefix** syntax is specified within curly braces proceeding the normal expected content of these element values and is composed of 3 optional parts:

<span style="font-weight:bold">{ <span style="color:green">Evaluation-Operator</span> | <span style="color:purple">Filter-Index-Range</span> | <span style="color:red">Filter-Path</span> } Regular-Assert</span>

The **NDJSON Assertion Prefix** syntax is applied in 4 stages:

<ol>
<li><span style="font-weight:bold; color:red">Filter-Path</span> evaluation. Example: <span style="font-weight:bold; color:red">.name[?(@.family=='Gracia')]</span> - all Patients whose name.family is 'Gracia'. A resource is included if the JSON-Path evaluation results in a <span style="font-weight:bold">Truthy</span> value (exists and is not false). If no <span style="color:red">Filter-Path</span> is specified then all resources filter through.</li>
<li><span style="font-weight:bold; color:purple">Filter-Index-Range</span>. Example: <span style="font-weight:bold; color:purple">1-10</span> - include resources indexed 1 through 10 in the assertion evaluation. If <span style="color:purple">Filter-Index-Range</span> is <span style="font-weight:bold">not</span> specified then all resources filter through. If <span style="color:red">Filter-Path</span> is specified then <span style="color:purple">Filter-Index-Range</span> operates on the resources that made it through the <span style="color:red">Filter-Path</span> and <span style="font-weight:bold">not</span> the original resources.</li>
<li><span style="font-weight:bold">Regular-Assert</span>. The Profile/Expression/Path/Resource evaluation on resources that made it past 1 and 2. Example: <span style="font-weight:bold">Patient</span> (if assertion is resource).</li>
<li><span style="font-weight:bold; color:green">Evaluation-Operator</span>. '<span style="font-weight:bold; color:green">any</span>' or '<span style="font-weight:bold; color:green">all</span>' - If '<span style="color:green">any</span>' operator is used then overall assertion evaluation passes if a single resource passes. The default is '<span style="color:green">all</span>'.</li>
</ol>

#### TestScript Example

The following TestScript provides a comprehensive list of asserts that show various usage examples of the **NDJSON Assertion Prefix** Syntax.

* [NDJSON Assertion Prefix Syntax Example](TestScript-ndjson-assertion-prefix.html)

<p id="publish-box">Here are some portions from this TestScript illustrating key examples of the <strong>NDJSON Assertion Prefix</strong> syntax.</p>

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

