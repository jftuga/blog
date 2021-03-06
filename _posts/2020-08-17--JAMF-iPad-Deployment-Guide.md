---
layout: post
title: "JAMF iPad Deployment Guide"
date: 2020-08-17
categories: ipad jamf sysadmin
tags: ipad jamf sysadmin
excerpt: A recipe book on how to perform various tasks in JAMF Cloud
---

These are my notes on deploying iPads with JAMF.  They only run internally and are locked down with a very limited number of apps.  The iPads are managed via JAMF Cloud.

___


## Import an unmanaged or new iPad into Apple
* Only Step
* * URL: [https://school.apple.com](https://school.apple.com)
* * Left: Device Assignments
* * Right: (1) Input iPad serial numbers (2) Action: Assign to Server: *Your Organization JAMF*

## Enroll an iPad into JAMF
This assumes that the iPad has been imported into this website: [https://school.apple.com](https://school.apple.com/)

* Only Step
* * Remove any preinstalled Apple Configurator profiles
* * Open Settings -> Reset -> Erase All Content & Settings
* * (this takes about 5 minutes to complete)

## Obtain Free or Purchased iPad Apps
* Only Step
* * URL: [https://school.apple.com](https://school.apple.com)
* * Left: Apps and Books
* * Middle: Search for the desired App
* * Right: Under `Buy Licenses` select a `Location` and `Quantity`
* * Right: Click `Get`
* In about a minute or two, you should see the App under the `Manage Licenses` section

## Log into JAMF
* * URL: https://YourOrgName.jamfcloud.com/
* * Username: *your username*
* * Password: *your password*

## Create a set of locations
It is best to use Buildings instead of Room because this allows you to use a drop-down selection box instead of free form text.

* Only Step
* * Left: Management Settings
* * Center: Network Organization
* * Right: Buildings
* * Right: New
* * Input a Display Name which can be a building, department, floor, etc.
* * Repeat as needed

## Create a group of iPad apps
A group may contain one or more apps

* Step 1
* * Left: Management Settings
* * Center: Network Organization
* * Right: Departments
* * Create an alphabetical, comma separated list of apps
* * * Example: Cardio Calc, PubMed

* Step 2
* * Left: Smart Device Group
* * Right: New
* * Tab: Mobile Device Group
* * Display Name: Dept XYZ, where XYZ is the exact name given in Step 1
* * * Example: Dept Cardio Calc, PubMed
* * Tab: Criteria
* * Right: Click Add
* * Criteria: Department
* * Operator: is
* * Value: the exact value given in Step 1
* * Right: Click Save

* Step 3
* * This may or may not need to be done, depending on the scope restrictions.
* * Left: Configuration Profiles
* * Right: YourOrgName Restrictions
* * Right: Scope
* *  * If the iPad apps no dot meet any of the Scope criteria, the Edit and Add in a the new Department

## Add a New App
* Step 1
* * Left: Mobile Device Apps
* * Under the `No category assigned` section, select your App
* * * Initially, this scope is: `No scope defined`
* Step 2a
* * Left: Under the `Scope` tab, select `Edit`
* Step 2b
* * Right: Click `Add`
* * Right: Select `Mobile Device Groups`
* * Right: `Add` your `Dept AppList`
* * Right: Click `Save`
* Step 3a
* * Right: Under `General` tab, select `Edit`
* Step 3b
* * Right: Under `Category`, select the appropriate category from the drop down
* * Right: Under `Distribution Method`, select `Install Automtically, Prompt User`
* * Right: Click `Save`
* Step 4a
* * Right: Under `Manage Distribution` tab, select `Edit`
* Step 4b
* * Right: Check `Assign Content Purchased in Volume`
* * Right: Select `Location` from drop down
* * Right: Click `Save`


## Add or Change an app group to an iPad
* Step 1
* * Left: search inventory 
* * Center: (click on your saved search )
* * Bottom: View tip: press Alt-V instead of clicking View
* * Click on the Device Name

* Step 2
* * Center: Under Inventory, click User and Location
* * Right: Edit
* * Right: under the Department drop-down, select the app group name
* * Right: (optional) under the Building, select the device location

## Check deployment status
* Step 1
* * Navigate to the iPad via the Search Inventory

* Step 2
* * Center: Management
* * View any Pending Commands
* * You may need to click Send Blank Push to nudge the deployment

## Add a website to the allowed content filter
* Step 1
* * Left: Configuration Profiles
* * Right: YourOrgName Content Filter  Main
* * Center: Content Filter
* * Right: Edit

* Step 2
* * Right: Click Add
* * Right: Input a URL and optionally a Title. If you input a Title, it will appear on the iPad as a bookmark
* * Right: Save

___

## Troubleshooting: Pending - Application is not available to install
* Step 1
* * Make a note of the problematic app under Deployment Status -> Management -> Pending Commands

* Step 2
* * Left: Management Settings
* * Center: Global Management
* * Right: Volume Purchasing
* * Center: Click on your organization name
* * Tab: Content
* * Right: click on the Refresh for the problematic app
* * * Note: This process may take a few minutes. It is recommended that you refresh no more than one app or eBook at a time.

* Step 3
* * After a few minutes, this should clear this error and your deployment should be pushed to your devices
* * See also: [https://www.jamf.com/jamf-nation/discussions/29863/pending-application-is-not-available-to-install](https://www.jamf.com/jamf-nation/discussions/29863/pending-application-is-not-available-to-install)

## Troubleshooting: A newly changed department will revert back to original department
* Step 1
* * Left: Management Settings
* * Center: Global Management
* * Right: Inventory Preload

* Step 2a
* * You can delete all active data and then the department should stick

* Step 2b
* * Alternatively, you can download the CSV file, edit with the correct department and the upload the modified CSV file.

## Troubleshooting: iPad will not connect to WiFi
* Only Step
* * The SSIDs with JAMF Cloud are case-sensitive.  Ensure the SSID you input matches the SSID exactly, including any capitalizations.
 
## Troubleshooting: iPad asks to accept WiFi certificate
* Only Step
* * Left: Configuration Profiles
* * Right: (click on your WiFi profile)
* * Center: Click Wi-Fi payload
* * Right: Trust tab
* * Right: Edit
* * Right: under Trusted Server Certificate Names, click Add
* * Right: Input the hostname that is serving the certificate. 
* * * Example: *radius.idm.school.edu*

## Troubleshooting: iPads can not upgrade to newest iOS version
* Updates will not run if the iPad battery is low
* Updates will not run if the there is low storage space
* * Settings -> General -> About -> Available
* A configuration profile has the following restriction enabled, which will need to be temporarily disabled:
* * Configuration Profiles -> *Profile Name* -> Restrictions -> Functionality -> Defer Software Update
* * This change must then be pushed to your iPads

