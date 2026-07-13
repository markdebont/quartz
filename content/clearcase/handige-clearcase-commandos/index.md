---
archive_status: raw_import_enriched
categories:
- geen-categorie
chapter: Werk en IT
content_kind: post
date: 2010-05-21
draft: false
publish: true
review_note: Nog inhoudelijk beoordelen vóór publicatie.
source_site: CLEARCASE.NL
source_type: wordpress_export
source_url: https://itsaclearcase.wordpress.com
tags:
- clearcase
- commands
- help
- ucm
themes:
- IT
- softwareontwikkeling
- vakontwikkeling
- persoonlijk archief
title: Handige Clearcase commando's
visibility: public
---

\*\*\* Make empty baseline

**makebl cleartool mkbl -ide -comp <comp>@\\<vob>** <format i.e. xxx\_10.050.5.0>

\*\*\* set baseline template

**cleartool chproj -blname\_template project,basename,date,time <project selector>**

\*\*\* Files importen

**clearfsimport -nsetevent -recurse -comment "VISI" d:\\Test\_000** (evt. -preview voor preview).

! **d:\\Test\_000** word dan mee geimporteerd !

\*\*\* Change Ownership of stream

**cleartool protect -chown <userID> <streamselector>**

\*\*\* Forward Merge Graphical merge, forward (to undelete files) make sure parent directory is enabled

\*\*\* show rebase history

**cleartool lshistory**

\*\*\* How to a Create symbolic link from the GUI

1) create activity 2) ga in bovenliggende source directory staan, right click en create Symbolink link 3) Kies de destination directory \*\*\* Snapshot view and hijacked files

To prevent hijacked files after updating/rebasing a snapshot view do not change casing in Clearcase filenames! Background information: The clearcase (file)system was originally developed on Unix where there is case sensitivity. This is somehow preserved. \*\*\* Renaming a UCM stream

Renaming the UCM stream is possible via the commandline interface. Use the following commands:

**cleartool rename brtype:old\_name new\_name** **cleartool chstream -generate old\_name** **cleartool rename stream:old\_name@\\pvob new\_name** **cleartool setcs -stream -tag your\_view\_name**

Note: the Project Explorer also has an option to rename when selecting a stream name. However, When you use this option however, only the stream name is changed, and not the underlaying branch name. When using this the stream- and branchname are decoupled Making it very hard to trace back changes in a branch. \*\*\* Rename a baseline

Get desciption of the oldbasline: **cleartool desc baseline:<old baseline>** and note the labeltype (example: "label status: Fully Labeled") connected to this baseline.

Second rename the baseline with **cleartool rename baseline:<old baseline> baseline:<new baseline>** and also rename the labeltype **cleartool rename lbtype::<old labeltype> baseline:<new labeltype>**

See also: Renaming a Baseline does not change the label type name --> http://www-01.ibm.com/support/docview.wss?uid=swg21251401 /

remark: If a baseline is already in use with a stream then it might happen that the synchronisation with the stream seams to be lost and there are no object visible in the explorer tree. To get the stream in sync again select a read-only interface from the streams configuration and change this to an older baseline. After this change the baseline of this read-only interface back to the intended baseline, and now all objects should be visible again. \*\*\* To remove an activity from a view:

remove all objects from the activity (check the empty UCM tab) uncheck the checkbox from "MyActivities" field in your view

From the command line **cleartool rmact activity:<ClearQuestNr>@\\<PVoB>**

Optionally in ClearQuest change the activity to the correct project / stream. \*\*\* remove private files

2 ways to remove your private files from a view:

1) Remove the view and recreate it (you will also lose checked out files) 2) From the command prompt in your view, issue following command:

**for /F "" %A in ('cleartool lsprivate -other') do del /F /Q %A**

NOTE: cleartool lsprivate also shows checked out files, so be careful! \*\*\* find stream from a baseline

Find the stream on which a baseline is created open a command line and type in:

**cleartool describe -long baseline:<baseline>@\\<PVOB> | find "stream:"**

Example: **cleartool describe -long baseline:<baseline>@\\<PVOB> | find "stream:"**

results in: **stream:<baseline>@\\<PVOB>**

\*\*\* Find delta between two baselines

**cleartool find . -version "{lbtype(<baseline\_reference\_1>) && !lbtype(<baseline\_reference\_2>)}" -exec "cleartool des -fmt "%En\\n" %CLEARCASE\_PN%"**

The command must be invoked from within a relevant view context \*\*\* Find changes since a particular date

**cleartool find . -version "{brtype(<stream\_path>) && created\_since(<date>)}" -exec "cleartool des -** **fmt "%En\\n" %CLEARCASE\_PN%"**

Example:

**cleartool find . -version "{brtype(\\main\\mboumans\_TSM\_UIM\_RcktC) && created\_since(13-Feb)}" -exec "cleartool des -** **fmt "%En\\n" %CLEARCASE\_PN%"**

The command must be invoked from within a relevant view context \*\*\* Eclipsed files

Use the Windows Explorer to delete the files. The eclipsed files will dis-appear in the Clearcase explorer.