Automatic installation
======================

The following procedure will prepare environment in VirtualBox
configured with port forwarding and shared folders automatically using
Vagrant tool. It's appropriate for testing and or development
installation on local workstation.

`Vagrant tool <https://www.vagrantup.com/downloads.html>`__ and
`Virtualbox <https://www.virtualbox.org/wiki/Downloads>`__ is required
for installing Repository from source codes. Tested on Vagrant (1.9.6
and 2.0.3) and VirtualBox (5.1.30 and 5.2.8).

External registration is needed with West-Life SSO and ARIA for
integrating single sign on and project proposal import. Follow
prerequisites at `Prerequisites <prerequisites.md>`__.

Execute following in your command line:

.. code:: text

    git clone https://github.com/h2020-westlife-eu/wp6-repository.git
    cd wp6-repository
    # (optionally unzip sp files from westlife sso and clientIds from ARIA) 
    # unzip westlifespfiles.zip
    vagrant up

After successful finish. The VM is prepared, the port 8080 is
automatically forwarded to VM's port 80. To connect to VM using SSH

``vagrant ssh``

The backend DB is using randomly generated access credentials at
``/etc/westlife/repository.key``

You may source this file by using
``source /etc/westlife/repository.key``

You may restart the backend service by
``service westlife-repository restart``
