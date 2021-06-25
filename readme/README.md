# SA-ESS-Windows
Splunk App for Enterprise Security and Windows Security log

***
**IMPORTANT**

Before installing version 5.0.0 please remove the older version you are running now. The installation directroy has changed from "SA-ESS-Windows" to "SA_ESS_Windows" this had to be done to make sure the app can be installed on 
Splunk Cloud. For apps that need to be installed in Splunk Cloud there needs to be a app id in the apps.conf [package] stanza, and the specs says:
> * id must be the same as the folder name in which your app lives in
>   $SPLUNK_HOME/etc/apps.
> * id must adhere to these cross-platform folder name restrictions:
>  * must contain only letters, numbers, "." (dot), and "_" (underscore)
>    characters.
***

# App goals
1. Fill the Enterprise Security Datamodels, with the right info. 
2. Remove unnecessary info from a lot of Windows events
3. Prevent the ingestion of admin passwords in case Microsoft Local Administrator Password Solution (LAPS) is used

# App content
The app contains: 
- eventtypes to get the correct tags on events for the datamodels.
- lots of props to get the correct fields for the different events for the datamodels.
- transforms to fix some things that are broken in $SPLUNK_HOME/etc/system/default/transforms.conf
- extended Windows eventlog lookup files to get more info on for example event 4625 (failed login)
- info via a lookup on the userAccountControl values

# System requirements
This app should run on all versions of Splunk, including Splunk Cloud.

# Installation
To benefit the removal of the unnecessary Windows event log data (the text that is below a lot of events) and to prevent the password ingestion, parts of the app need to be moved to either the Indexer or a Heavyforwarder depending on your enviroment.
From the props.conf get the SEDCMD rows per sourcetype and place them in a separate props.conf and install that on your Indexer or your HeavyForwarder. 

The SEDCMD rows are below the following stanzas:
```
[source::ActiveDirectory]
[(?::){0}WinEventLog...]
```
# Configuration
After installation some macros and eventtypes might need to be changed depending on your enviroment.

## Macros
- winevent_index        : The index where the Windows Eventlogs are. default: `(index=wineventlog)`
- admon_index           : The index where the admon events are. default: `(index=ad)`
- security_log          : The Windows Security eventlogs. default: `source=WinEventLog:Security`
- system_log            : The Windows System eventlogs. default: `source=WinEventLog:System`
- application_log       : The Windows Application eventlogs. default: `source=WinEventLog:Application`

## Eventtypes
- winevent_security     : The Windows Security eventlogs. default: `(index=wineventlog source="WinEventLog:Security")`
- winevent_system       : The Windows System eventlogs. default: `(index=wineventlog source="WinEventLog:System")`
- winevent_application  : The Windows Application eventlogs. default: `(index=wineventlog source="WinEventLog:Application")`