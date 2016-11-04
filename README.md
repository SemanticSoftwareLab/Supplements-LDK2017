# Supplementary Materials for our ICSC 2017 Submission
This repository contains the supplementary materials used to reproduce the
experiments submitted to the [11th International Conference
on Semantic Computing (IEEE ICSC 2017)](http://icsc.eecs.uci.edu/2017/).

## Knowledge Base
The provided knowledge base contains all the extracted entities from our dataset of 90 PeerJ Computer Science articles. The semantic triples are expressed using the Resource Description Framework (RDF) syntax.

Additionally, the knowledge base contains two exemplary user profiles:
* A *Researcher* profile that represents the user profile of a computer science professor, populated from the recent publications of the paper's second author.
* A *Leaner* profile that represents the user profile of a computer science PhD student, populated from the recent publications of the paper's first author.

### Vocabularies
The triples in our knowledge base use a number of (linked open) vocabularies:

| Vocabulary/Ontology    | Namespace in KB | Description   | URL  |
| ------------- |:-------------:|:-----|:-------------|
| Publication Ontology (PUBO) | pubo | PUBO vocabularies are used to express the relations between documents, annotations and their inter-relations. | http://lod.semanticsoftware.info/pubo/pubo |
| SALT Rhetorical Ontology (SALT) | sro | Used for describing rhetorical elements (claims and contributions) of documents. | http://salt.semanticauthoring.org/ontologies/sro   |
| Resource Description Framework (RDF) | rdf | Formal representation of semantic triples. | http://www.w3.org/1999/02/22-rdf-syntax-ns |
| RDF Schema (RDFS) | rdfs | Schema used in our knowledge base, as well as DBpedia ontology. | http://www.w3.org/2000/01/rdf-schema |
| Content Ontology | cnt | Used for representing the literal (verbatim) content of extracted annotations.| http://www.w3.org/2011/content |
| User Modeling Ontology | um | Used for formal representation of scholar user profiles. | http://intelleo.eu/ontologies/user-model/ns/ |
| User Competence Management | c | Used for modeling users' competences as well as their competence records (i.e., provenance information).| http://www.intelleo.eu/ontologies/competences/ns/ |
| Friend-of-a-Friend (FOAF) | foaf | Used for traversing the DBpedia ontology from resources to their corresponding Wikipedia pages. | http://xmlns.com/foaf/0.1/|
| DBpedia Ontology | dbpedia | Used for grounding documents' topics with DBpedia ontology resources. | http://dbpedia.org/resoource/|
| Dublin Core Terms | dcterms | Used for traversing the semantic categories of DBpedia ontology. | http://purl.org/dc/terms/|

### Sample Triples
![alt text](https://github.com/SemanticSoftwareLab/Supplements-ICSC2017/raw/master/graphics/sample_triples.png "Sample Triples from KB")

### Publishing the knowledge base through Fuseki
The knowledge base can be published with [Apache Jena Fuseki](https://jena.apache.org/documentation/serving_data/) that can servce RDF data over HTTP. 

1. You need to [download](https://jena.apache.org/download/#apache-jena-fuseki) Fuseki server first. Follow the instructions on the download page for installation. The experiments descibed in our paper are based on Fuseki version 2.0.0. 
1. The knowledge base triples generated in our experiments are located in the [knowledge-base](../master/knowledge-base) folder. Unzip the [triples.zip](../master/knowledge-base/triples.zip) file to your disk and publish it through Fuseki using either of the following approaches:

   a) You can create an empty dataset with Fuseki and upload the .nq file using the "upload files" tab.

   b) Alternatively, you can use Apache Jena's [TDB-loader command](https://jena.apache.org/documentation/tdb/commands.html#tdbloader) to copy the triples to a directory on your disk and publish the directory through Fuseki:

   ```/JENA_INSTALLATION/bin/tdbloader --loc=/PATH/TO/TRIPLESTORE /PATH/TO/triples.nq```
   
1. After publishing the knowledge base, verify that all of the triples are uploaded. A triple count on the knowledge base should return 1,623,868 triples.

### License

The knowledg-base (triples.zip) is distributed under the terms of the [Creative Commons Attribution License v4.0](https://creativecommons.org/licenses/by/4.0/). You can find a copy of the license in the [knowledge-base](../master/knowledge-base) folder.

## Queries
The queries discussed in this section are provided to reproduce the figures in the Application section of our paper. The SPARQL queries can be found in the [queries](../master/queries) folder.

### Scenario 1: Summarizing Relevant Articles
The goal of this scenario is to find documents that mention a specific topic (i.e., named entity) in their content. To this end, we aim to retrieve all the topics (i.e., named entities) within rhetorical zones of a given document and then perform a federated query to the DBpedia ontology in order to expand the search for relevant documents from the dataset.

Execute [Query 1](../master/queries/query1.rq) against the Fuseki endpoint to see the results.

### Scenario 2:  Curating a Personalized Reading List
The goal of Scenario 2 is to personalize the list of document retrieved from the query in [Scenario 1](#scenario-1-summarizing-relevant-articles) and order them with respect to how many topics in each document matches against a given user's competences in his profile (in this user, the researcher profile).

Executing [Query 2](../master/queries/query2.rq) against the knowledge base will return an integer number that can be used to rank the result set from the first query.

### Scenario 3: Filling the Knowledge Gap of Learners
In Scenario 3, we incorporate our learner user profile in the query. The goal here is to find all topics in a document that the user does not know about and bring in additional information (e.g., a brief description and online sources) from Linked Open Data ontologies (DBpedia ontology in our case).

Execute [Query 3](../master/queries/query3.rq) against the Fuseki endpoint to see the results.
