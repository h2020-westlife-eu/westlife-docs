Data Management
===============

Research data is acquired, interpreted, published, reused, and sometimes
eventually discarded. Structural biology has a strong tradition of data
sharing, expressed by the founding of the Protein Data Bank (PDB) in
1971. In 2015, 9338 new structures were deposited in the Protein Data
Bank, which is the result of approximately more than 25,000 experiments.
All these experiments have together a combined data rate greater than
that of the Large Hadron Collider.

One of the main obstacles to fully achieve a proper handling of the data
life cycle in structure biology is managing the data, which will include
datasets acquired in a range of different experimental facilities, some
easy to transfer by email or USB stick, and some so large that it is
only feasible to process them at source.

To address this obstacles, `Virtual Folder <virtual-folder/>`__ provides
common interface to access scattered data among data infrastructures
offered by facilities and common data infrastructures offered by
scientific community (EUDAT, Indico, and others now in upcoming (EOSC)
European Open Science Cloud effort) or commercial vendors (Dropbox,
Google Drive, ...). Virtual Folder is provided as public service without
any other installation as well as configuration for `private
deployment <virtual-machines.md>`__ is available for proccessing data
using common software suites for structural biology in local
workstation, cluster or user's prefered cloud computing infrastructure.

For experimental facilities that are newly embarking on data management,
we provide a reference implementation of a `Repository <repository/>`__
that supplies suitable metadata to the portal, follows common workflow
of project as defined ARIA and allows deposit datasets via Virtual
Folder to desired data infrastructure. Note that this reference
installation expects to be ammended based on the feedback of facility
users thus technology choosen for backend and frontend implementation
are described and source code provided in order to facilitate further
customization and development.
