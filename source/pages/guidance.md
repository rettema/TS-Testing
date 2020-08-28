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

{% include link-list.md %}
