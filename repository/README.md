# Repository

This is reference implementation of a repository that supplies suitable metadata to the portal.

Main features of this repository implementation is that it:

* Scientist are authenticated by West-Life single-sign-on - authentication of users to this repository is made by public web service
* it's possible to authorize or restrict access to users authenticated by West-Life SSO to gain access to this repository 
* supports Instruct ARIA workflow, facility reservation system, scheduling and managing visit to tackle experiment - Repository installation can obtain user's registered project and bind them to dataset.
* Facility Staff are authenticated by repository itself by separated staff account management.    

## Installation

Installation can be made on physical server launching bootstrap.sh script - supported on RHEL 7 derivative. Tested on Scientific Linux 7 and Centos 7 systems. Further details at [Installation Guide](installation-guide/)

## Developer's Guide

There are described some non-standard integration steps with ARIA API,made in this implementation. Read further details at [Developers-guide](developers-guide/)

## History

- 18.04 - Initial version was developed within work package WP6 by the [West-Life H2020 project](https://west-life.eu) running from 2015 to 2018.
- 18.11 - This release contains new features for metadata generation and view and some minor security updates.