# Manual installation from source codes

This procedure is appropriate for deploying into existing server or machine. E.g. within dedicated cloud infrastructure. It is recommended to use Scientific Linux 7.x, Centos 7.x or other RHEL 7.x derivatives without modification (other systems needs some modification in bootstrap script).

External registration is needed with West-Life SSO and ARIA for integrating single sign on and project proposal import. Follow prerequisites at [Prerequisites](https://h2020-westlife-eu.gitbook.io/virtual-folder-docs/repository/installation-guide/prerequisites).

Log in into the server as root and do following:

```bash
git clone https://github.com/h2020-westlife-eu/wp6-repository.git
cd wp6-repository
bootstrap.sh
```

Follow bootstrap.sh messages for intermittent errors. Once bootstrap.sh finishes succesfully, the server is prepared to be used. The web server is configured at http://localhost or at your FQDN. You may customize apache web server configuration at `/etc/httpd/conf.d/`.



