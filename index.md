---
layout: lesson
root: .
---

<!-- USING THIS LESSON TEMPLATE -->

<!--
1. UPDATE THE INFORMATION IN _data/info.yml
2. UPDATE THE INDEX OF LESSONS IN _data/lessons.yml
-->

<!-- THE LESSON INFORMATION -->

{% for info in site.data.info %}

#Data Carpentry {{ info.topic }} for {{ info.domain }}

Data Carpentry's aim is to teach researchers basic concepts, skills,
and tools for working with data so that they can get more done in less
time, and with less pain. The lessons below were designed for those interested 
in working with {{info.domain %}} data in {{info.topic %}}. 

<!-- This block displays the contributors' names. -->

**Content Contributors: {{info.contributors | join: ', ' %}}**


<!-- This block displays the lesson maintainers' names. -->

**Lesson Maintainers: {{info.maintainers | join: ', ' %}}**

<br> 

####Lesson status: {{ info.status }} 
  [Information on Lesson Status Categories]()

<!-- INDEX OF LESSONS ON THIS TOPIC -->

## Lessons:

{% for lesson in site.data.lessons %}

- [{{ lesson.name }}]({{ lesson.url }})

{% endfor %}


## Data

Data files for the workshop are available at: ({{info.dataurl %}})[{{info.dataurl %}}]


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


{% endfor %}



