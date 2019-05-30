Docker-compose yaml file for [TEITOK](http://www.teitok.org/index.php?action=home), using a slightly modified version of TEITOK dockerfile by [egon w. stemle](https://gitlab.inf.unibz.it/commul/docker/teitok) of UniBozen (Sud Tirol, Europe).

TEITOK is a web-based application for digital philologists and corpus linguists, combining philological TEI annotations with the powerful query processor Open Corpus WorkBench (CWB).

For the alternative version with CQPweb, see rahonalab/CQPweb-docker.

## INSTALLATION
1. Drop a copy of TEITOK in teitokcwb/ directory. You need access to TEITOK's private repository
2. Edit .env and secrets.env to fit your needs
3. docker-compose build
4. docker-compose up -d
5. Using the persistent volume on your filesystem, pick one of the template in ./${FIRSTNAME}-container/webapp/TEITOK/projects and copy it in ./${FIRSTNAME}-container/webapp/TEITOK/ as your project name.
6. Visit http://youraddress/TEITOK/yourproject

## Persistent volume
In order to mantain your TEITOK files and Apache2 configuration through further code upgrades, two persistent volumes are provided:

| Host | Container:directory | Description & Usage |
| ------ | ----- | ------------------- |
| ./${FIRSTNAME}-container/webapp | TEITOK:/var/www/html | where TEITOK/ sits |
| ./${FIRSTNAME}-container/conf | TEITOK:/conf | apache2 configuration |
