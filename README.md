### Access standard knowledge indexed from code repositories, connected to the Linked Open Data access points

[![Deploy to GitHub Pages](https://github.com/vemonet/shapes-of-you/workflows/Deploy%20website/badge.svg)](https://github.com/vemonet/shapes-of-you/actions?query=workflow%3A%22Deploy+website%22) [![CodeQL analysis](https://github.com/vemonet/shapes-of-you/workflows/CodeQL%20analysis/badge.svg)](https://github.com/vemonet/shapes-of-you/actions?query=workflow%3A%22CodeQL+analysis%22) 


🖥 Access the web app at **[index.semanticscience.org](https://index.semanticscience.org)**

📬 Query our knowledge graph using the OpenAPI at **[grlc.io/api-git/vemonet/shapes-of-you/subdir/api](http://grlc.io/api-git/vemonet/shapes-of-you/subdir/api)** (powered by [grlc.io](http://grlc.io) and [SPARQL](https://www.w3.org/TR/sparql11-query/))

✨ Directly query the **<a href="http://yasgui.triply.cc/#query=PREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20dc%3A%20%3Chttp%3A%2F%2Fpurl.org%2Fdc%2Felements%2F1.1%2F%3E%0APREFIX%20dcterms%3A%20%3Chttp%3A%2F%2Fpurl.org%2Fdc%2Fterms%2F%3E%0APREFIX%20schema%3A%20%3Chttps%3A%2F%2Fschema.org%2F%3E%0APREFIX%20sh%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fshacl%23%3E%0APREFIX%20shex%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fshex%23%3E%0APREFIX%20void%3A%20%3Chttp%3A%2F%2Frdfs.org%2Fns%2Fvoid%23%3E%0ASELECT%20DISTINCT%20%3Fshape_type%20(count(distinct%20%3Frepository)%20AS%20%3Frepos_count)%20(count(distinct%20%3Fshape_file)%20AS%20%3Ffile_count)%0AWHERE%20%7B%20%0A%20%20%20%20%3Fshape_file%20a%20schema%3ASoftwareSourceCode%20%3B%0A%20%20%20%20%20%20%20%20a%20%3Fshape_type%20%3B%0A%20%20%20%20%20%20%20%20dc%3Asource%20%3Frepository%20.%0A%20%20%20%20FILTER(%3Fshape_type%20!%3D%20schema%3ASoftwareSourceCode)%0A%7D%20GROUP%20BY%20%3Fshape_type&endpoint=https%3A%2F%2Fgraphdb.dumontierlab.com%2Frepositories%2Fshapes-registry&requestMethod=GET&tabTitle=Shapes%20of%20you%20query&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table">SPARQL endpoint on YASGUI</a>**. The SPARQL endpoint is also conveniently accessible in the webapp **Active endpoints** tab, since Shapes of You indexes its own SPARQL query files, and computes metadata for its SPARQL endpoint.

**Shapes of you** is a global index for semantically descriptive files published to public Git repositories ([GitHub](https://github.com), [GitLab](https://gitlab.com), and [Gitee](https://gitee.com/)), it enables semantic web enthusiast to connect those standard knowledge definitions to active Linked Open Data access points (SPARQL endpoints).

To be found by our indexer, make sure your repository description, or topics, on [GitHub](https://github.com), [GitLab](https://gitlab.com), or [Gitee](https://gitee.com) includes one of the resources mentionned below, we automatically index files from public repositories everyday at 1:00 GMT+1 🕐

* **SHACL shapes**: we index RDF files such as `.ttl`, `.rdf`, `.jsonld`, etc), with all `sh:NodeShape` they contain
* **ShEx expressions**: we index `.shex` files, and ShEx shapes defined in RDF files
* **SPARQL queries**: we index `.rq` and `.sparql` files, and parse [grlc.io](http://grlc.io) APIs metadata
* **OWL ontologies**: we index all RDF files with all `owl:Class`  they contain
* **SKOS vocabularies**: we index all RDF files with all `skos:Concept` they contain
* **RML mappings**: we index RDF files, with all `r2rml:SubjectMap` and `rml:LogicalSource` they contain
* **R2RML mappings**: we index RDF files, with all `r2rml:SubjectMap` they contain
* **[CSVW](https://www.w3.org/TR/tabular-data-primer/) metadata**: we index RDF files, with all `csvw:Column` they contain
* **Nanopublication templates**: we index RDF files, with all `nt:AssertionTemplates` and inputs they contain
* **OBO ontologies**: we index all `.obo` files with all terms they contain
* **OpenAPI specifications**: we index `.yml`, `.yaml` and `.json` files, and parse the spec to retrieve API metadata
* **DCAT datasets**: we index RDF files, with all `dcat:Dataset` they contain

If your repository or endpoint is missed by our indexer:

* Additional GitHub repositories in the file [`etl/EXTRAS_GITHUB_REPOSITORIES.txt`](https://github.com/vemonet/shapes-of-you/blob/main/etl/EXTRAS_GITHUB_REPOSITORIES.txt)

* Additional SPARQL endpoints in the file [`etl/EXTRAS_SPARQL_ENDPOINTS.txt`](https://github.com/vemonet/shapes-of-you/blob/main/etl/EXTRAS_SPARQL_ENDPOINTS.txt) 

## Technical overview 🧭

This web service is composed of those 4 main parts, described more in details below:

* A python script to retrieve SPARQL queries, SHACL & ShEx Shapes files with some metadata from GitHub repositories. The retrieved data is defined using [RDF](https://www.w3.org/RDF/).
  * A [GitHub Actions worklows](https://github.com/vemonet/shapes-of-you/actions?query=workflow%3A%22Deploy+to+GitHub+Pages%22) runs everyday at 1:00 and 13:00 to execute the python script, and publish the RDF output to the triplestore
* A React web app written in TypeScript, which displays the files and metadata from the SPARQL endpoint with filters, and search
  * The website is automatically deployed by [GitHub Actions worklows](https://github.com/vemonet/shapes-of-you/actions?query=workflow%3A%22Deploy+to+GitHub+Pages%22) to [GitHub Pages](https://index.semanticscience.org) at each push to the `main` branch.
  * We use [expo](https://expo.io/) to build this [Progressive Web App](https://web.dev/progressive-web-apps/) (aka. PWA), it can be installed as a native app on any computer desktop (using Chrome is recommended), or smartphones.
* A triplestore with a publicly available SPARQL endpoint at https://graphdb.dumontierlab.com/repositories/shapes-registry
* A grlc.io powered OpenAPI to query the SPARQL endpoint at http://grlc.io/api-git/vemonet/shapes-of-you
  * Most SPARQL queries used by the webapp are also provided as API calls 

![Shapes of You architecture](/website/assets/shapes-of-you-architecture.png)

---

## Data model 📋

We defined and published a simple schema for our data as a OWL ontology, mainly re-using schema.org concepts.

Checkout the OWL ontology in [`website/assets/shapes-of-you-ontology.ttl` 🦉](/website/assets/shapes-of-you-ontology.ttl)

Here is an overview of the ontology (generated by [gra.fo](https://gra.fo/)):

![Ontology overview](/website/assets/shapes-of-you-ontology.png)

### Prefixes

Just copy/paste this if you are missing some prefixes to query the Shapes of You knowledge graph:

```SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX dct: <http://purl.org/dc/terms/>
PREFIX dcat: <http://www.w3.org/ns/dcat#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX sio: <http://semanticscience.org/resource/SIO_>
PREFIX schema: <https://schema.org/>
PREFIX sh: <http://www.w3.org/ns/shacl#>
PREFIX shex: <http://www.w3.org/ns/shex#>
PREFIX void: <http://rdfs.org/ns/void#>
PREFIX void-ext: <http://ldf.fi/void-ext#>
PREFIX sdm: <https://w3id.org/vocab/sdm#>
PREFIX r2rml: <http://www.w3.org/ns/r2rml#>
PREFIX rml: <http://semweb.mmlab.be/ns/rml#>
PREFIX nt: <https://w3id.org/np/o/ntemplate/>
PREFIX csvw: <http://www.w3.org/ns/csvw#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
```

### Classes

* "Shape" files: `schema:SoftwareSourceCode`
  * Properties:
    * `dcterms:hasPart`
    * `rdfs:comment`
    * `schema:codeRepository` > `schema:DataCatalog`
  * Subclasses:
    * `sh:Shape` (SHACL shape)
    * `shex:Schema` (ShEX schema)
    * `sh:SPARQLFunction` (SPARQL query) - additional properties: `void:sparqlEndpoint`, `schema:query`
    * `owl:Ontology` (OWL ontology)
    * `skos:ConceptScheme` (SKOS vocabulary)
    * `sio:000623` (OBO ontology)
    * `schema:APIReference` (OpenAPI)
    * `rml:LogicalSource` (RML and YARRRML mappings)
    * `r2rml:TriplesMap` (R2RML mappings)
    * `nt:AssertionTemplate` (Nanopublication templates)
    * `dcat:Dataset` (DCAT datasets)
* Git repositories: `schema:DataCatalog`
  * Properties:
    * `rdfs:comment`
* Active SPARQL endpoints:`schema:EntryPoint`

---

## Run the web app 🛩️

Requirements:  [npm](https://www.npmjs.com/get-npm) and [yarn](https://classic.yarnpkg.com/en/docs/install/#debian-stable) installed.

### In development 🏗

Clone the repository:

```bash
git clone https://github.com/vemonet/shapes-of-you
cd shapes-of-you
```

Install dependencies :inbox_tray:

```bash
yarn
```

Run the web app on http://localhost:19006, it should reload automatically at each changes to the code :arrows_clockwise:

```bash
yarn dev
```

Upgrade the packages versions in `yarn.lock` 🔒

```bash
yarn upgrade
```

### In production 🌍

This website is automatically deployed by a [GitHub Actions worklow](https://github.com/vemonet/shapes-of-you/actions?query=workflow%3A%22Deploy+to+GitHub+Pages%22) to GitHub Pages which is accessed from http://index.semanticscience.org

You can also build locally in the `/web-build` folder and serve on http://localhost:5000 (checkout the `Dockerfile`)

```bash
yarn build
yarn serve
```

Or run directly using [Docker :whale:](https://docs.docker.com/get-docker/) (requires [docker installed](https://docs.docker.com/get-docker/))

```bash
docker-compose up
```

> Checkout the [docker-compose.yml](/docker-compose.yml) file to see how we run the Docker image.

---

## Index all the knowledge ⛏️

Requirements: Python 3.6+, git

### Index files from code repositories 🗃️

This script is run every day by the mighty [`.github/workflows/index-shapes.yml`](https://github.com/vemonet/shapes-of-you/blob/main/.github/workflows/index-shapes.yml) workflow 

The Python script retrieves shapes files from various popular Git services API (GitHub GraphQL API, GitLab API , Gitee API), and generates RDF data. The RDF data is then automatically published to the publicly available triplestore by the GitHub workflow.

You can find the python scripts and requirements in the [`etl`](https://github.com/vemonet/shapes-of-you/tree/main/etl) folder.

Use this command to locally define the `GITHUB_TOKEN` and `GITLAB_TOKEN` **environment variables required** to run the script (you might need to adapt on Windows, but you should know better than me):

```bash
export API_GITHUB_TOKEN=MYGITHUBTOKEN000
export GITLAB_TOKEN=MYGITLABTOKEN000
export GITEE_TOKEN=MYGITEETOKEN000
```

> Add those commands to your `.zshrc` to make it permanent (or `.bashrc` if you're nostalgic of the 90's)

For GitHub you can create a new GitHub API key (aka. personal access token) at https://github.com/settings/tokens

Then install requirements:

```bash
pip3 install -r etl/requirements.txt
```

Retrieve shapes files from search the [GitHub GraphQL API](https://developer.github.com/v4/explorer) (here using a topic to search):

```bash
python3 etl/index_shapes.py github topic:shacl-shapes
```

Retrieve shapes files from [GitLab API](https://docs.gitlab.com/ee/api/) using the [`python-gitlab` package](https://pypi.org/project/python-gitlab/):

```bash
python3 etl/index_shapes.py gitlab sparql
```

Retrieve shapes files from [Gitee API](https://gitee.com/api/v5/swagger#/getV5SearchRepositories):

```bash
python3 etl/index_shapes.py gitee ontology
```

### Generate SPARQL endpoints metadata ✨

This task is performed every day by the swifty [`.github/workflows/analyze-endpoints.yml`](https://github.com/vemonet/shapes-of-you/blob/main/.github/workflows/analyze-endpoints.yml) workflow 

We use the [`d2s`](https://github.com/MaastrichtU-IDS/d2s-cli) tool (aka. data2services) to generate [HCLS metadata](https://www.w3.org/TR/hcls-dataset/) for a SPARQL endpoint:

```bash
d2s metadata analyze https://graphdb.dumontierlab.com/repositories/shapes-registry -o metadata.ttl
```

We commit the generated metadata file to the `metadata` branch

## Contribute 👩‍💻

Contributions are welcome! See the [guidelines to contribute](/CONTRIBUTING.md).

## Acknowledgements 🤝

RDF data hosted in a [Ontotext GraphDB triplestore](https://graphdb.ontotext.com/)

OpenAPI powered by [grlc.io](http://grlc.io)

Ontology built with [gra.fo](https://gra.fo)

Data processing workflows run for free using [GitHub Actions](https://github.com/features/actions) open source plan

Files parsed using python libraries: `rdflib`, `obonet`, `prance`