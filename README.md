# hetio-net
CSCI 493.71: Big Data  
Project I: Model of HetioNet

Khan_Rafi: Khinshan Khan and Shakil Rafi

## Requirements

- Python 3+
- mongo 4+
- neo4j 3.5+
- Java 8
- pymongo 3.9.0
- py2neo 4.3.0

## Setup

The following instructions were tested on Arch Linux:

- start the MongoDB service:
    - `sudo systemctl enable mongodb.service`
- start the neo4j service:
  - `sudo systemctl start neo4j.service`
- in the file `/etc/neo4j/neo4j.conf` make sure that `dbms.directories.import` is set to `/var/lib/neo4j/import`.
- the directory `/var/lib/neo4j/import/` should exist and your user should have read and write access to it:
    - by default, this directory will be owned by the `neo4j` user and the `neo4j` group
    - `usermod -a -G neo4j $(whoami)`
- the default neo4j username is `neo4j` and the password is `password`
    - modify `utils/common.py` to match your neo4j username and password

## Run

From download:

```bash
cd Khan_Rafi
python app.py
```

From GitHub:

```bash
git clone https://github.com/kkhan01/hetio-net
cd hetio-net
python app.py
```

## Notes

This project models Hetionet to answer the following queries:

1. Given a disease, what is its name, what are drug names
that can treat or palliate this disease, what are gene
names that cause this disease, and where this disease
occurs? Obtain and output this information in a single
query.
    - This is done by creating a MongoDB database that stores every disease in
    a document with its relevant information

1. Supposed that a drug can treat a disease if the drug or
its similar drugs up-regulate/down-regulate a gene, but
the location down-regulates/up-regulates the gene in
an opposite direction where the disease occurs. Find all
drugs that can treat new diseases (i.e. the missing
edges between drug and disease). Obtain and output
the drug-disease pairs in a single query.
    - This is done by creating a neo4j database representing a graph of Hetionet and
    then querying the relevant paths

### Diagrams

#### Query 1
![](https://media.discordapp.net/attachments/356260638294540289/642094055508934656/unknown.png?width=361&height=245)
#### Query 2
![](https://media.discordapp.net/attachments/356260638294540289/642095049727016988/unknown.png?width=702&height=443)
![](https://media.discordapp.net/attachments/356260638294540289/642093988056137785/unknown.png?width=581&height=360)
### edges.tsv relationships

#### Compound
- CrC = Compound Resembles Compound
- CtD = Compound Treats Disease
- CpD = Compound Palliates Diseases
- CuG = Compound Upregulates Genes
- CbG = Compound Binds Genes
- CdG = Compound Downregulates Gene
#### Disease
- DrD = Disease Resembles Disease
- DlA = Disease Localizes Anatomy
- DuG = Disease Upregulates Gene
- DaG = Disease Associates Genes
- DdG = Disease Downregulates Genes
#### Anatomy
- AuG = Anatomy Upregulates Genes
- AeG = Anatomy Expresses Gene
- AdG = Anatomy Downregulates Genes
#### Gene
- Gr>G = Gene Regulates Gene
- GcG = Gene Covaries Gene
- GiG = Gene Interacts Gene
