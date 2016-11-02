# Supplementary Materials for our ICSC 2017 Submission
This repository contains the supplementary materials used to reproduce the
experiments submitted to the [11th International Conference
on Semantic Computing (IEEE ICSC 2017)](http://icsc.eecs.uci.edu/2017/).

## Knowledge Base

### Publishing the knowledge base through Fuseki
The knowledge base can be published with [Apache Jena Fuseki](https://jena.apache.org/documentation/serving_data/) that can servce RDF data over HTTP. 

1. You need to [download](https://jena.apache.org/download/#apache-jena-fuseki) Fuseki server first. Follow the instructions on the download page for installation. The experiments descibed in our paper are based on Fuseki version 2.0.0. 
1. The knowledge base triples generated in our experiments are located in the [knowledge-base](../master/knowledge-base) folder. Unzip the [triples.zip](../master/knowledge-base/triples.zip) file to your disk and publish it through Fuseki using either of the following approaches:

   a) You can create an empty dataset with Fuseki and upload the .nq file using the "upload files" tab.

   b) Alternatively, you can use Apache Jena's [TDB-loader command](https://jena.apache.org/documentation/tdb/commands.html#tdbloader) to copy the triples to a directory on your disk, and publish the directory through Fuseki:
   
1. more stuff



## Queries


