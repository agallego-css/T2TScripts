# Exchange Online: Cross-tenant migration functions

*Any sample script/function in this repository is provided AS IS and not supported under any Microsoft standard support program, service and without warranty of any kind.*

## Overview

T2TScripts repository contains functions to sync all necessary attributes between the source and target tenant before the MRS move mailbox and perform all necessary updates to the source and target object once the migration is completed. Before starting using the resources provided in this repository, please review the [Microsoft official document about the cross-tenant EXO migration](https://docs.microsoft.com/en-us/microsoft-365/enterprise/cross-tenant-mailbox-migration). It’s very important that you understand how the migration works in order to use these functions.


## Installation

 Open a Windows Powershell with "Run as Administrator" and run:
``` powershell
Install-Module T2TScripts -Force
```

If you face installation issues such as *Unable to resolve package source* you can run the following before install the module:
``` powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

Once the module is installed, you can run either of the commands and its parameters:
``` powershell
Export-T2TAttributes
Import-T2TAttributes
Update-T2TPostMigration
Export-T2Tlogs
```

If you want to check for module updates you can run:
``` powershell
Find-Module T2TScripts
```

If there is any newer version than the one you already have, you can run:
``` powershell
Update-Module T2TScripts -Force
```


## Timeline

The EXO cross-tenant migration involves many steps. You can refer to the following timeline to understand how the **T2TScripts** should be used:

![T2TScripts – Timeline EXO migration](https://user-images.githubusercontent.com/43185536/115865890-35f0d980-a439-11eb-857f-783643424dcd.png)

Refer to the following links to review the parameters, requirements and instruction for each function:

- [Export-T2TAttributes](/T2TScripts/functions/Export-T2TAttributes.md)

- [Import-T2TAttributes](/T2TScripts/functions/Import-T2TAttributes.md)

- [Update-T2TPostMigration](/T2TScripts/functions/Update-T2TPostMigration.md)

- [Export-T2TLogs](/T2TScripts/functions/Export-T2TLogs.md)


## How it works

1 - Fill some Exchange custom attributes (1-15) with a specific value on the mailboxes that you want to migrate. This will become the filter for the function to get only mailboxes with the attribute that you choose.

2 - Understand all parameters that you may use to run the functions.

3 - From the source environment, run the `Export-T2TAttributes` function to dump the source attributes and validate yourself that the CSV generated by the function contains only remote mailboxes filtered by the custom attribute used in the step 1.

4 - From the target environment, run the `Import-T2TAttributes` function to create Mail User objects (aka MEU) in the target on-prem AD based on the CSV created in the step 3.

5 - Start the migration batch to move the mailboxes refered in the last steps.

7 - Once the migration batch reaches 95% as Synced status, run the `Update-T2TAttributes -Destination` in the destination environment. You will be required to provide the same CSV used in the step 3 and 4. Once the functions finishes, it will export a new CSV called MigratedUsers.csv containing the results.

8 - From the source environment, run the `Update-T2TAttributes -Source`. The function will require the MigratedUsers.csv generated in the step 7.


## Version History

[Change Log](/T2TScripts/changelog.md)