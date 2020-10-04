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

Touchstone has the ability for a user to create and use a Test System that is OAuth2 enabled, and with that comes of few features that are important to take note of when it comes to Test Executions performed against a Test System that has an OAuth2 Authorization service connected to it.

### Fixture Ids

With the addition of a more robust OAuth2 environment into Touchstone, there are a few new fixtures, and values under those fixtures, that can be reached for test script authors.

* One new fixture is the _dest1SmartConfig_ (_dest2SmartConfig_, _dest3SmartConfig_, etc.). The fixture allows the test script author to access different variables coming from the JSON document that is at the _.well-known/smart-configuration.json_ endpoint. It acts the same as retrieving variables from the capabilities statement. For example, to use _dest1SmartConfig_ to retrieve the Client ID, you would use the <span style="color:red">sourceId</span> and <span style="color:red">path</span> in the testscript as <span style="color:red">sourceId</span> = dest1SmartConfig, <span style="color:red">path</span> = $.clientId.

* Test Systems have special Oauth2 values that can be be retrieved by test script authors. The fixtures have a naming convention of _dest1SystemConifg_, _dest2SystemConfig_, _origin1SystemConfig_, _origin2SystemConfig_, etc. These fixtures have the following attributes:

<table border="1" cellspacing="0" cellpadding="5" style="border-collapse:collapse;border:solid windowtext 1pt">
	<thead>
		<tr>
			<td style="font-weight:bold">Fixture ID</td>
			<td style="font-weight:bold">Description</td>
		</tr>
	</thead>
	<tr>
		<td>name</td>
		<td>The Test System name</td>
	</tr>
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

{% include link-list.md %}
