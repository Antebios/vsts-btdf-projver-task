# Description

VSTS build task to modify the BTDF Project file (.btdfproj) that creates the actual MSI that is migrated.

This will take the version number source of your choosing, for me it was GitVersion, and update the BTDF Project file **ProductVersion** and generate a new random GUID for the **ProductId**.

Your choice of version numbers can come from:

* Build Number variable Build.BuildNumber
* Environment Variable of your choice
* GitVersion properties, specifically *MajorMinorPatch* and combines *CommitsSinceVersionSource*

## Why

MSI packages have a limit of Major.Minor.Path, but with [Sematic Version](https://semver.org) and GitVersion I can generate a version number like "1.2.3-ci4" or "1.2.3+4".  Translating that into a version number compatible with MSI and being upgradable (to be compatible with the BTDF Framework) is a little tricky.  I solved this problem with a PowerShell script that I have to place in each and every git repository.  I think it was best to convert this into an extension that everyone can use that has had to deal with the same problem.  And I can finally stop needing to require to have the powershell script in each BizTalk interface git repo.

So, the versions cited before both become "1.2.30004".

## Why the padding of zeros?

MSI Patch number has a limitiation value of 65,535, which is 5 characters long.  So I take the "3" and "4" and fill the remaining space with zeros.

Icons made by [Those Icons](https://www.flaticon.com/authors/those-icons) from [Flaticon](https://www.flaticon.com/) licensed by [Creative Commons BY 3.0](http://creativecommons.org/licenses/by/3.0/)
