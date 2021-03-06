# Description

Azure DevOps build task to modify the BTDF Project file (.btdfproj) that creates the actual MSI that is migrated.

This will take the version number source of your choosing, for me it was GitVersion, and update the BTDF Project file **ProductVersion** and generate a new random GUID for the **ProductId**.

Your choice of version numbers can come from:

* Build Number variable Build.BuildNumber
* Environment Variable of your choice
* GitVersion properties, specifically *MajorMinorPatch* and combines *CommitsSinceVersionSource*

![BTDFProjVerForm](images/marketplace/btdfprojver-1.png)

## Why

MSI packages have a limit of Major.Minor.Path, but with [Sematic Version](https://semver.org) and [GitVersion](https://gitversion.readthedocs.io/en/latest/) I can generate a version number like "1.2.3-ci4" or "1.2.3+4".  Translating that into a version number compatible with MSI and being upgradable (to be compatible with the BTDF Framework) is a little tricky.  I solved this problem with a PowerShell script that I have to place in each and every git repository.  I think it was best to convert this into an extension that everyone can use that has had to deal with the same problem.  And I can finally stop needing to require to have the powershell script in each BizTalk interface git repo.

So, the versions cited before both become "1.2.03004".

## Why the padding of zeros?

MSI Patch number has a limitiation value of 65,535, which is 5 characters long.  So I take the "3" and "4" and fill the remaining space with zeros.

## Update to 0.1.9
When using GitVersion and tagging a release, the produced MSI should have a clean version "1.2.3", but instead is incorrectly being set to "1.2.3.4" where "4" is the pre-release or commits since last tagged.  But when using tags in conjunction with GitVersion this is deemed a "blessed" release artifact and should be reflected in the naming convention of the msi, thus no need for a fourth component to the version number.
