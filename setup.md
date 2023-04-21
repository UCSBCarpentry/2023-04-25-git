

> ## Setup Checklist
> To participate in this workshop, you will need access to software as described
> below. In addition, you will need an up-to-date web browser. We maintain a list
> of common issues that occur during installation as a reference for instructors
> that may be useful on the <a href="{{site.swc_github}}/workshop-template/wiki/Configuration-Problems-and-Solutions">Configuration
> Problems and Solutions wiki page</a>.
> If your device is logged into any drives, you need to log out before starting the setup instructions (e.g. OneDrive, DropBox, etc.)
> - The Bash Shell
> - Git
> - [Download lesson data](https://github.com/swcarpentry/shell-novice/raw/gh-pages/data/shell-lesson-data.zip)
{: .checklist}

> ## Data Download
> For this workshop, we will be using instructional sample data that was created for this workshop.
> The data can be found on the workshop repository:
> [Link to download data](https://github.com/swcarpentry/shell-novice/raw/gh-pages/data/shell-lesson-data.zip).
> Once the download is complete, you will have a ZIP file of a folder named "shell-lesson-data".
> Please <strong>unzip this file and move the folder, "shell-lesson-data", onto your desktop.</strong>
{: .prereq}



{% if site.carpentry == "swc" %}
{% include swc/setup.html %}
{% elsif site.carpentry == "dc" %}
{% include dc/setup.html %}
{% elsif site.carpentry == "lc" %}
{% include lc/setup.html %}
{% elsif site.carpentry == "incubator" %}
Please check the "Setup" page of
[the lesson site]({{ site.incubator_lesson_site }}) for instructions to follow
to obtain the software and data you will need to follow the lesson.
{% endif %}
