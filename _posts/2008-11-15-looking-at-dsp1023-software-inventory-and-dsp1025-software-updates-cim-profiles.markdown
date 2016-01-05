---
layout: post
title: Looking at DSP1023 (Software Inventory) and DSP1025 (Software Updates) CIM
  profiles
date: '2008-11-15'
tags:
- cim
- newsuse
- rpm
- software
- suse
---

Yesterday I sat together with [Stefan Haas][5] in front of the whiteboard and analyzed the specification of the [DSP1023 (Software Inventory)][1] and [DSP1025 (Software Updates)][2] [CIM profiles][4], from the [DMTF][3].

Our goal was to understand it, and therefore we tried to map the concept to the knowledge we already have, which is Linux software management (which in turn can be reduced almost to packages :-). Here are our notes:

(For simplicity, I am skipping the CIM\_ prefix for all classes)

### Packages

Packages are represented as usual NVRs (name, version, release) using the SoftwareIdentity class.

The url of the package is represented associating the SoftwareIdentity with url instances (SoftwareIdentityResources) trough a SAPAvailableForElement association. Products, subpackages and components can be modeled by using OrderedComponent class, which references the component and one member per instance.

One can associate hardware (or any managed element) with software using ElementSoftwareIdentity

### Repositories

Repos are modeled by using SystemSpecificCollection and associated via HostedCollection to a system (that seems to be the local computer).

Packages are associated to the repository via the MemberOfCollection class.

Installed packages are represented by instances of InstalledSoftwareIdentity (which references the ComputerSystem where it is installed).

### Groups, Patterns

Just another software identity (for the group itself), and references the grouped identities using OrderedComponent instances (which has two fields, the GroupComponent and PartComponent, which reference the respective instances).

### Dependencies

If the target software does not have and instance, a SoftwareIdentity is created and isEntity set to false (kind of named dependency)  
For dependencies that exists software collection, then OrderedDependency is instantiated for each dependency and Antecedent and Dependent is filled)

For installing the packages, a SoftwareInstallationService is needed.  
Packages can be installed calling installFromSoftwareIdentity() (name, version release instance) or installFromByteStream() (like an rpm package).

To define an installation service, the SoftwareInstallationService class need to specify what it supports by instantiating SoftwareInstallationServiceCapabilities (which has properties like the supported URI schemes), and then this capabilities are associated back to the service using ElementCapabilities.

To define whether a package is compatible with a given target, one uses the TargetTypes array in the SoftwareIdentity class.

If a SoftwareIdentity is available and there is some SoftwareInstallationService that is compatible (or capable) of installing it, a instance of the ServiceAffectsElement needs to be instantiated.

Before actually installing or updating, the client checks if a SoftwareIdentity can be installed on a element, using the CheckSoftwareIdentity() method on the SoftwareInstallationService. You give this method the software itself, the collection and the target element. You get back a InstallCharacteristics with the details (for example if you need to reboot after installing or not).

Once you start the operation using InstallFromSoftwareIdentity(), you get back a Job instance that represent the task. Also true for InstallFromByteStream(), but here you pass an Image instance instead of a SoftwareIdentity. There is a similar InstallFromURI operation too. (jobs are only returned if the service has async capabilities).

The client can pass options to the install operation using InstallOptionValues.

TODO: figure out ElementSoftwareStatus more.

Additionally, you can [see how HP has implemented the inventory profile with rpm][6]. It has some useful information about which rpm tags belong to which classes properties for example.

[1]: http://www.dmtf.org/standards/published_documents/DSP1023_1.0.0.pdf  
[2]: http://www.dmtf.org/standards/published_documents/DSP1025.pdf  
[3]: http://www.dmtf.org  
[4]: http://www.dmtf.org/standards/profiles/  
[5]: http://en.opensuse.org/User:Haass  
[6]: http://docs.hp.com/en/5992-1936/ar01s03.html

