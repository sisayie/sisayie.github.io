---
title: 'Creating Issues for Projects in GitLab'
date: 2024-11-23
permalink: /posts/2024/11/creating-issues-for-projects-in-GitLab/
tags:
  - GitLab
  - Issue
  - Automation
---

Creating issues  en mass in GitLab (using the web interface) is sometimes tidious or may require unnecessary context switching. This lab is to show how you can create bulk of issues using python.

Requirements
======

1) You need to install python-gitlab library (https://python-gitlab.readthedocs.io/en/stable/gl_objects/issues.html#project-issues) in order to perform more complex logic during issue creation

2) You need to create a project and get its id. 

3) You need to have personal access token.
! pip install python-gitlab

Steps
======

1) Authentication
------
Provide your personal access token and the Gitlab instance url. You can generate a token with the necessary scopes (e.g., API) from your GitLab profile settings.

2) Project Selection
------
Provide the ID of the project where you want to create the issue. You can find the project ID in the project's URL (see screenshot below).

3) Creating the Issue
------
The new issue uses dictionary and defines the title, description, and labels for the new issue. You can customize these fields as needed. Once the dictionary is populated with the necessary information, the project.issues.create(new_issue) method creates the issue and returns an Issue object. The full code for creating a new issue is shown below.

Code
======

# import required library
import gitlab

# Authenticate to GitLab
gl = gitlab.Gitlab('https://your_gitlab_instance_url', private_token='your_private_token')

# Select the project
project = gl.projects.get(project_id='your_project_id')

# Create a new issue
new_issue = {
    'title': 'New Issue Title',
    'description': 'This is a new issue description.',
    'labels': ['bug', 'frontend']
}

issue = project.issues.create(new_issue)

print(f"New issue created: {issue.title}")

How (and from where) do you get project id? You can get project id by clicking the 3 dots after Fork on Gitlab web interface as shown in the screenshot below. As soon as you click the three dots, you will see the popup that reads "Copy project ID: <id>". Clicking on the popup will copy the project id to clipboard. You can then paste it in ```project = gl.projects.get(id='your_project_id')``` to replace the placeholder ```your_project_id```.
<img src="how_to_get_project_id.png" alt="drawing" width="600"/>

You can extend the above code by incorporating the following: 
- Error Handling: Consider adding error handling mechanisms to handle exceptions like authentication failures, API rate limits, or network errors.
- Asynchronous Operations: For large-scale operations, you might want to explore asynchronous programming techniques to improve performance.
- Advanced Features: The gitlab library offers many more features, such as assigning issues to users, setting due dates, and closing issues. Refer to the library's documentation for detailed usage.