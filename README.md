# Supplementary Materials for our LDK 2017 Submission
This repository contains the supplementary materials used to reproduce the
experiments submitted to the [Language, Data and Knowledge Conference (LDK 2017)](http://ldk2017.org).

#### Table of Contents
1. [Knowledge Base](#knowledge-base)
  1. [Vocabularies](#vocabularies)
  2. [URI Schema](#uri-schema)
  3. [Sample Triples](#sample-triples)
  4. [Publishing the Knowledge base](#publishing-the-knowledge-base-through-fuseki)
  5. [License](#license)
2. [Queries](#queries)
  1. [Service 1](#service-1-summarizing-relevant-articles)
  2. [Service 2](#service-2--curating-a-personalized-reading-list)
  3. [Service 3](#service-3-filling-the-knowledge-gap-of-learners)
  

## Knowledge Base
The provided knowledge base contains all the extracted entities from our dataset of 100 PeerJ Computer Science articles. The semantic triples are expressed using the Resource Description Framework (RDF) syntax.

### URI Schema
| Entity | URI Schema | Example | Note |
| ------------- |:-------------:|:-----|:-------------|
| Article | `http://tmp/peerj-cs-100/cs-NN.xml` | `http://tmp/dataset/cs-12.xml` | Replace `NN` with {1..98,100,101} |
| User | `http://semanticsoftware.info/lodexporter/author/NAME` | `http://semanticsoftware.info/lodexporter/author/carlos+j.-corrada-bravo` |  Replace `NAME` with lower-cased, hypehanted full name of the author |


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
The following figure shows an example subgraph from our populated knowledge base. At the top we have a document that was processed by the agent. The left half of the graph shows the triples used to represent the user profile of `ex:Bahar` who is an author of paper `ex:Doc#9`. The topics extracted from the document, e.g. `ex:Competence#4` represents a topics that the user is competent in. The right half of the graph shows the semantic representation of the document by agent. As the graph shows, papers and user's can be interlinked through common topic (i.e., named entities in papers and competences in user profiles) instances in the knowledge base.

![Sample Triples from KB](https://github.com/SemanticSoftwareLab/Supplements-ICSC2017/raw/master/graphics/sample_triples.png "Sample Triples from KB")

### Publishing the knowledge base through Fuseki
The knowledge base can be published with [Apache Jena Fuseki](https://jena.apache.org/documentation/serving_data/) that can servce RDF data over HTTP. 

1. You need to [download](https://jena.apache.org/download/#apache-jena-fuseki) Fuseki server first. Follow the instructions on the download page for installation. The experiments descibed in our paper are based on Fuseki version 2.0.0. 
1. The knowledge base triples generated in our experiments are located in the [knowledge-base](../master/knowledge-base) folder. Unzip the [triples.zip](../master/knowledge-base/triples.zip) file to your disk and publish it through Fuseki using either of the following approaches:

   a) You can create an empty dataset with Fuseki and upload the .nq file using the "upload files" tab.

   b) Alternatively, you can use Apache Jena's [TDB-loader command](https://jena.apache.org/documentation/tdb/commands.html#tdbloader) to copy the triples to a directory on your disk and publish the directory through Fuseki:

   ```/JENA_INSTALLATION/bin/tdbloader --loc=/PATH/TO/TRIPLESTORE /PATH/TO/triples.nq```
   
1. After publishing the knowledge base, verify that all of the triples are uploaded. A triple count on the knowledge base should return 1,281,971 triples.

### License

The knowledge-base (triples.zip) is distributed under the terms of the [Creative Commons Attribution License v4.0](https://creativecommons.org/licenses/by/4.0/). You can find a copy of the license in the [knowledge-base](../master/knowledge-base) folder.

## Queries
The queries discussed in this section are provided to reproduce the figures in the Application section of our paper. The SPARQL queries can be found in the [queries](../master/queries) folder.

### Service 1: Summarizing Relevant Articles
The goal of this scenario is to find documents that mention a specific topic (i.e., named entity) in their content. To this end, we aim to retrieve all the topics (i.e., named entities) within rhetorical zones of a given document and then perform a federated query to the DBpedia ontology in order to expand the search for relevant documents from the dataset.

Execute [Query 1](../master/queries/query1.rq) against the Fuseki endpoint to see the results. Note that Query 1 contains a federated query to the DBpedia SPARQL endpoint and might take a while to execute depending on the endpoint status.

### Service 2:  Curating a Personalized Reading List
The goal of Scenario 2 is to personalize the list of document retrieved from the query in [Scenario 1](#scenario-1-summarizing-relevant-articles) and order them with respect to how many topics in each document matches against a given user's competences in his profile (in this user, the researcher profile).

Executing [Query 2](../master/queries/query2.rq) against the knowledge base will return an integer number that can be used to rank the result set from the first query.

### Service 3: Filling the Knowledge Gap of Learners
In Scenario 3, we incorporate our learner user profile in the query. The goal here is to find all topics in a document that the user does not know about and bring in additional information (e.g., a brief description and online sources) from Linked Open Data ontologies (DBpedia ontology in our case).

Execute [Query 3](../master/queries/query3.rq) against the Fuseki endpoint to see the results.
