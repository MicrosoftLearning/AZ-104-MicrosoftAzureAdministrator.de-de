---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
---

# <a name="content-directory"></a>Inhaltsverzeichnis

Erforderliche Dateien für Labs können [HIER](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip) heruntergeladen werden.

Links zu den einzelnen Lab-Übungen sind nachfolgend aufgelistet.

## <a name="labs"></a>Labs

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modul | Labor |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} – {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}


