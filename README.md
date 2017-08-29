macos profile
=============

[![Build Status](https://travis-ci.org/ANTS-Framework/macos_profile.svg?branch=master)](https://travis-ci.org/ANTS-Framework/macos_profile)

Description
------------
Install macOS profiles from template or remove installed profiles by identifier.

It role works by dynamically creating mobileconfig files from templates and installing/uninstalling
them using the macos `profiles` command.

Profile Identifiers are generated from `{{ macos_profile__identifierPrefix }}.{{ mobileconfig.template }}`

If you want to know more about profiles, there is the official
[Configuration Profile Reference](https://developer.apple.com/library/content/featuredarticles/iPhoneConfigurationProfileRef/Introduction/Introduction.html)
by Apple but most of all, the mac admin community has some great resources on github. See
[this repo](https://github.com/clburlison/profiles) by [clburlison](https://github.com/clburlison)
for a great collection of profiles and have a look at [profiledocs](https://mosen.github.io/profiledocs/index.html)
by [mosen](https://github.com/mosen). You can also generate profiles from mxc using
[mcxToProfile](https://github.com/timsutton/mcxToProfile) by [timsutton](https://github.com/timsutton).

Some of the profile templates have been taken from these
repositories. Namely:

* skipsetup.j2 [rtrouton](https://github.com/rtrouton/profiles) and [gregneagle](https://github.com/gregneagle/profiles)
* safari.j2 [nmcspadden](https://github.com/nmcspadden/Profiles/blob/master/Safari.mobileconfig)
* menuextras.j2 [nmspadden](https://github.com/nmcspadden/Profiles/blob/master/MenuExtra.mobileconfig)
* disableautoupdates.j2 [gregneagle](https://github.com/gregneagle/profiles/tree/master/autoupdate_disablers)
* airport.j2 [gregneagle](https://github.com/gregneagle/profiles/blob/953d33d39fd3243dbbc64829b4300668df73b511/mcxairport.mobileconfig)

Role Variables
--------------

Default variables for profiles are listed in their corresponding defaults file: `defaults/{{ mobileconfig.template }}.yml`

E.g. all variables available for the "safari" profile are listed in `defaults/safari.yml`.
Set the variables as needed in group\_vars or host\_vars.

There are a few variables that are used in every template:

```yml
macos_profile__identifierPrefix: "com.pretendcorp.it.macos"
macos_profile__PayloadOrganization: "Pretend Corp"
macos_profile__PayloadRemovalDisallowed: "true"
# Save files to this directory
macos_profile__destinationFolder: "/usr/local/pretendcorp/profiles"
```


Example Playbook
----------------

You have to call the role for every profile you want to install.
Only the profiles with names corresponding to `mobileconfig.template`
are installed/uninstalled.

```yml
# Manage Finder and set interface level to simple
- hosts: macos_public
  vars:
    - macos_profile__finder_InterfaceLevel: "Simple"
    - mobileconfig: { template: "finder", state: "present" }
  roles:
  - role: macos_profile

# Make sure energysaver profile is not installed
- hosts: macos_no_energy
  vars:
    - mobileconfig: { template: "energysaver", state: "absent" }
  roles:
  - role: macos_profile
```

License
-------

GPLv3

Author Information
------------------
Part of the [ANTS Framework](https://ants-framework.github.io/)
