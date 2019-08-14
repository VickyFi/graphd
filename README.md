# graphd
Graph database for multi-omics data

# Steps for populating the database and running the server
1. Change the directory
```bash
cd demo 
```
2. Download and install Neo4J
```bash
wget https://neo4j.com/artifact.php?name=neo4j-community-3.5.8-unix.tar.gz

tar -zxvf neo4j-community-3.5.8-unix.tar.gz
```

3. Move the data files to neo4j folder
```bash
mv *.gz neo4j-community-3.5.8/

cd neo4j-community-3.5.8

mkdir data/databases/demo.db
```

4. Import data files using neo4j-import command
```bash
./bin/neo4j-import --into data/databases/demo.db/ --nodes case.nodes.gz --nodes gene.nodes.gz --nodes sample.nodes.gz --relationships case_sample.edges.gz --relationships sample_gene.edges.gz
```


5. Start the neo4j server and setup the password to 'SOME PASSWORD'
```bash
 ./bin/neo4j start
```
* Open a browser and go to URL http://localhost:7474/
* Change the default password to neo4j to SOME PASSWORD.

6. Extract the clinical files and update the graph database with clinical information. Also edit the updateClinical.py and change the password from PASSWORD to 'SOME PASSWORD'

```bash
gunzip clinical.gz
python updateClinical.py -c demo/neo4j-community-3.5.8/clinical
``` 

7. Example Query
* In the browser, Enter the following query 
```bash
$ match p=(c:Case)-[hs:HAS_SAMPLE]-(s:Sample)-[:HAS_EXPRESSION]->(g:Gene) return p limit 5

# TO stop the server, do:
bin/neo4j stop
```
