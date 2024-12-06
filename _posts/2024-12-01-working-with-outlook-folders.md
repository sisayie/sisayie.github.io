---
title: 'Automating Folder Management in outlook'
date: 2024-12-01
permalink: /posts/2024/12/Automating-Folder-Management-in-outlook/
tags:
  - Automation
  - Outlook
  - folder management
---
Folder management can be daunting, especially when you need to keep folders in multiple systems in sync. I have always been creating folders under subfolder in Inbox folder in outlook. As shown in the example structure below, every time I start working on a project or proposal, I create a folder in initation. As the developement process continues and its status changes, I move the folder to a corresponding folder. For example, if the proposal is in its initial discussion phase, it will have a folder under `Initiation`; if the proposal development continues, the folder is moved to `Preparation` and if it is submitted, the folder is moved to `Submitted and follow up`; if it is rejected, the folder is moved to `Rejected`; if it is accepted, it does not belong to proposal category any more and the folder is moved to `Running` projects.

I do this in my email folder (then create rules for email routing), in my file system (to organize files) as well as browser bookmark (to organize relevant websites). Doing it manually is a bit tidious and error prone.  As a result, I decided to automate the entire workflow. This blog post explains how I automate the creating of folders in my outlook mail box.

```
user_name@domain.com
    Inbox
        Projects
            1_Proposal
                11_Initiation
                    <project_names>
                12_Preparation
                13_Submitted and follow up
                14_Rejected
            2_Running
                <project_names>
            3_Completed
```
Remember, if you want to create a subfolder under an existing subfolder of the Inbox in Outlook, you need to first access the subfolder and then create a new folder under it.

To create a subfolder in the "Inbox" folder of Outlook using Python, you can utilize the `pywin32` library, which allows Python to interact with the Outlook application through COM (Component Object Model) automation. So, the first step is to install the pywin32 library using `pip install pywin32`. The code below shows the implementation of creating a subfolder under a given parent folder.

```
import win32com.client

def create_subfolder_under_subfolder(parent_folder_name, subfolder_name):
    '''
    Description: Function to create a subfolder under an existing subfolder in Inbox.
    
    Inputs: the subfolder you want to create and the parent folder in which you want to create your subfolder

    Usage: create_subfolder_under_subfolder(parent_folder_name, subfolder_name)

    '''
    # Initialize the Outlook application
    outlook = win32com.client.Dispatch("Outlook.Application")
    
    # Get the Namespace object (MAPI)
    namespace = outlook.GetNamespace("MAPI")
    
    # Get the Inbox folder
    inbox = namespace.GetDefaultFolder(6)  # 6 is the constant for Inbox
    
    # Find the parent subfolder under Inbox
    try:
        parent_folder = inbox.Folders[parent_folder_name]
    except KeyError:
        print(f"Subfolder '{parent_folder_name}' does not exist under Inbox.")
        return
    
    # Create the new subfolder under the parent folder
    subfolder = parent_folder.Folders.Add(subfolder_name)
    print(f"Subfolder '{subfolder_name}' created under '{parent_folder_name}'.")

# Example usage
parent_folder_name = "subfolder_name"  # Name of the existing subfolder under Inbox
subfolder_name = "NewSubFolder"  # Name of the new subfolder to be created
create_subfolder_under_subfolder(parent_folder_name, subfolder_name)
```

- The function will initialize Outlook Application with `win32com.client.Dispatch("Outlook.Application")`, allowing access to its functionality. 
- Namespace (MAPI): We access the MAPI interface, which gives us the ability to interact with Outlook data such as emails, folders, etc.
- Get Inbox Folder: The `GetDefaultFolder(6)` method retrieves the Inbox folder (6 is the default constant for the Inbox).
- Create Subfolder: The `Folders.Add(folder_name)` method adds a new folder with the specified name under the Inbox.

Now, you can only focus on maintaining your file containing the list of proposals/projects you work on, e.g., excel file, json file or a mere text file. I suggest you create cron job that runs on regular basis to check the content of the file you maintain and compare it with the directory structure of your mailbox, bookmark and filesystem and update if needed, relieving you from doing the same thing again and again.

**Assumptions**:
- Outlook is installed and running on your system.
- The script will create the folder within the Outlook profile that is currently logged in.

----
If you have questions or comments, you can reach me via [LinkedIn](www.linkedin.com/in/sisayie).