Docker-compose yaml file for [TEITOK](http://www.teitok.org/index.php?action=home), using a slightly modified version of TEITOK dockerfile by [egon w. stemle](https://gitlab.inf.unibz.it/commul/docker/teitok) of UniBozen (Sud Tirol, Europe).

TEITOK is a web-based application for digital philologists and corpus linguists, combining philological TEI annotations with the powerful query processor Open Corpus WorkBench (CWB).

For the alternative version with CQPweb, see 

## INSTALLATION
1. Drop a copy of TEITOK in teitokcwb/ directory. You need access to TEITOK's private repository
2. docker-compose build
3. docker-compose up -d
4. Visit http://youraddress/TEITOK/myproject and follow the instructions
5. Enjoy! And thank Maarten Jansen for his wonderful software!
