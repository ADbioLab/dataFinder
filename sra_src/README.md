# NCBI Metadata Scraper

Repository for scraping metadata from NCBI xml run records. Script uses a `taxonomy_query` from the `params.json` to get the correct taxid scientific names for use in the final DB search (currently only SRA). Additional search parameters for the search can be passed with the `final_query` in `params.json` and will be appended to the taxonomy IDs with an AND clause.

For information on formating the NCBI Taxonomy database search please see the [documentation](https://www.ncbi.nlm.nih.gov/books/NBK53758/).

The final NCBI search query will be formed with the following format: `(organism-1 [ORGANISM] OR organism-1 [ORGANISM]) <final_query>`. Please see [SRA](https://www.ncbi.nlm.nih.gov/sra/docs/srasearch/) for how to build a search term (currently only support DB). 

**Note-** Utilizes the [Entrez Direct](https://www.ncbi.nlm.nih.gov/books/NBK179288/) and queries (e.g. 'metagnome' search with no other filters) that return lots of SRA runs can cause timeout errors (**TODO**: handle this issue). 

## Prerequisites

- Set up a `params.json` in the upper directory. with the following attributes:

```json
{
    "entrez_email": "<your email>",
    "entrez_api": "<your NCBI API key>",
    "taxonomy_query": "<the NCBI taxonomy database query for getting the correct organisms IDs for the SRA search>",
    "search_type": "<NCBI DB to be searched, only SRA supported>",
    "final_query": "<additional search terms to be appended to the organism IDs from the taxonomy query>",
    "id_file": "<name.txt file for saving the SRA query ids>"
}
```

# Running 
Below gives details on how to run the script with or withour docker.

## Without Docker

1. Set up the python env.

```bash
uv venv name -python 3.12
uv source /path/to/venv/bin/activate
uv pip install -r requirements.txt
```

2. Run the script.

```bash
python runner.py
```

## With Docker (WIP)
Warning- data will not be shared with host machine and will be lost when the docker container finishes. Currently cannot be used.

1. Build the image.
```bash
docker build -t sra_xml_reader .
```

2. Run the docker container.
```bash
docker run -t sra_xml_reader
```