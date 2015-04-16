---
layout: lesson
root: .
---

<!-- USING THIS LESSON TEMPLATE -->

<!--
All the lesson specific information is in the following two files.
1. UPDATE THE INFORMATION IN _data/info.yml
2. UPDATE THE INDEX OF LESSONS IN _data/lessons.yml
-->

<!-- THE LESSON INFORMATION -->

<!-- Get the information from _data/info.yml -->
{% for info in site.data.info %}

#Data Carpentry {{ info.topic }} for {{ info.domain }}

Data Carpentry's aim is to teach researchers basic concepts, skills,
and tools for working with data so that they can get more done in less
time, and with less pain. The lessons below were designed for those interested 
in working with {{info.domain %}} data in {{info.topic %}}. 


**Content Contributors: {{info.contributors | join: ', ' %}}**


**Lesson Maintainers: {{info.maintainers | join: ', ' %}}**

<br> 

####Lesson status: {{ info.status }} 
  [Information on Lesson Status Categories]()


<!-- ###### INDEX OF LESSONS ON THIS TOPIC ###### -->

## Lessons:

<!-- Get information from _data/lessons.yml -->

{% for lesson in site.data.lessons %}

- [{{ lesson.name }}]({{ lesson.url }})

{% endfor %}

<!-- End information from _data/lessons.yml -->

## Data

Data files for the workshop are available at: ({{info.dataurl %}})[{{info.dataurl %}}]


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



{% if page.topic == "Python" %}
{% include pythonSetup.html %}
{% else %}
{% include anySetup.html %}
{% endif %}

<p><strong>Twitter</strong>: @datacarpentry


{% endfor %}



