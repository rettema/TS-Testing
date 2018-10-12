---
title: Implementation Guide HomePage
layout: default
active: terminology
---

{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

* Do not remove this line (it will not be displayed)
{:toc}


<!-- end TOC -->

## Introduction

The AEGIS Touchstone Testing Implementation Guide is based on [FHIR Version R4] and defines the minimum conformance requirements for processing of optional external Rules Engine logic.  

## Actors

The following actors are part of the AEGIS Touchstone Testing IG:

* FHIR Test Engine: An application that implements the [Testing FHIR]({{site.data.fhir.path}}testing.html) framework.
* External Rules Engine: An application that can be invoked by the FHIR Test Engine when processinge FHIR TestScript assert actions.


## Profiles

The list of AEGIS Touchstone Testing Profiles is shown below.

{% include list-simple-profiles.xhtml %}


## Extensions

The list of AEGIS Touchstone Testing Extensions is shown below.

{% include list-simple-extensions.xhtml %}

----


Authors: Richard Ettema, Tareq Nabeel

{% include link-list.md %}
