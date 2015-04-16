---
layout: lesson
root: .
lastupdated: April 16, 2015
contributors: ["First Last", "First Last", "Pls Add Others"]
maintainers: ["First Last", "First Last"] 
domain: Domain Name
topic: Topic
dataurl: 

---

<!-- USING THIS LESSON TEMPLATE -->

<!--
1. UPDATE THE INFORMATION ABOVE
2. UPDATE THE INDEX OF LESSONS IN _includes/lesson-index.html
-->


#Data Carpentry {{page.topic %}} for {{page.domain %}}

<!-- This block displays the contributors' names if they are available. -->
{% if page.contributors %}
  **Content Contributors:**
  {{page.contributors | join: ', ' %}}
{% endif %}

<!-- This block displays the lesson maintainers' names if they are available. -->
{% if page.maintainers %}
  **Lesson Maintainers:**
  {{page.maintainers | join: ', ' %}}
{% endif %}

Data Carpentry's aim is to teach researchers basic concepts, skills,
and tools for working with data so that they can get more done in less
time, and with less pain. The lessons below were designed for those interested 
in working with {{page.domain %}} data in {{page.topic %}}. 

<!-- INDEX OF LESSONS ON THIS TOPIC -->

## Lessons:

{% include lesson-index.html %}

Data files for the workshop are available at: ({{page.dataurl %}})[{{page.dataurl %}}]

<em>Updates will be posted to this website as they become available.</em>

<p>&nbsp;

<h2>Requirements</h2>

<p>
Data Carpentry's teaching is hands-on, so participants are encouraged to use
their own computers to insure the proper setup of tools for an efficient workflow.
<em>These lessons assume no prior knowledge of the skills or tools</em>, but working 
through this lesson requires working copies of the software described below.
To most effectively use these materials, please make sure to install everything 
<em>before</em> working through this lesson.
</p>



{% if page.topic == "Python" %}
{% include pythonSetup.html %}
{% else %}
{% include anySetup.html %}
{% endif %}

<p><strong>Twitter</strong>: @datacarpentry




