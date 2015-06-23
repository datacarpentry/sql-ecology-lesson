---
layout: lesson
root: .
lastupdated: April 16, 2015
contributors: ["First Last", "First Last", "Pls Add Others"]
maintainers: ["First Last", "First Last"]
domain: Domain Name
topic: Topic
software: 
dataurl:
status: Under Development
---

<!-- USING THIS LESSON TEMPLATE -->
<!-- Lesson specific information is taken from the YAML header at the top of the page -->

<!-- THE LESSON INFORMATION -->

<!-- Get the information from _data/info.yml -->

#Data Carpentry {{ page.topic }} for {{ page.domain }}

Data Carpentry's aim is to teach researchers basic concepts, skills,
and tools for working with data so that they can get more done in less
time, and with less pain. The lessons below were designed for those interested 
in working with {{page.domain %}} data in {{page.topic %}}. 


**Content Contributors: {{page.contributors | join: ', ' %}}**


**Lesson Maintainers: {{page.maintainers | join: ', ' %}}**

<br> 


####Lesson status: {{ page.status }} 
<!--
  [Information on Lesson Status Categories]()
-->

<!-- ###### INDEX OF LESSONS ON THIS TOPIC ###### -->

## Lessons:


1. [Lesson 00 Name](00-lesson-00-name.html)
2. [Lesson 01 Name](01-lesson-01-name.html)


## Data

Data files for the workshop are available here: ({{page.dataurl %}})[{{page.dataurl %}}]


<br>

<h2>Requirements</h2>

<p>
Data Carpentry's teaching is hands-on, so participants are encouraged to use
their own computers to insure the proper setup of tools for an efficient workflow.
<em>These lessons assume no prior knowledge of the skills or tools</em>, but working 
through this lesson requires working copies of the software described below.
To most effectively use these materials, please make sure to install everything 
<em>before</em> working through this lesson.
</p>



{% if page.software == "Python" %}
{% include pythonSetup.html %}
{% elsif page.software == "Spreadsheets" %}
{% include spreadsheetSetup.html %}
{% elsif page.software == "R" %}
{% include rSetup.html %}
{% else %}
{% include anySetup.html %}
{% endif %}

<p><strong>Twitter</strong>: @datacarpentry





