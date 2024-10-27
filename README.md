# aml_assignment

# 26-10-2024

In order to overcome the problem of incomplete data, we decided to take matters into our own hands and scrape the data from the EOL website, without a thought for ethical and legal considerations. The justification for this is that this data is available freely on the web, such usage is in line with their terms (see: https://api.eol.org/docs/what-is-eol/terms-of-use), and that we need this information for a relatively small scale (only 500 species).

We were able to obtain traits for almost all of the species IDs, for traits that describe it further like 'locomotion', 'mass', and traits that can relate it to other species like 'preyed upon', 'are eaten by' etc.

The corresponding code and information can be foundin /scraping

# 25-10-2024

Investigating the completeness of traits available in traits.csv. 
This file was downloaded from here: https://opendata.eol.org/dataset/all-trait-data-large/resource/6e24f0df-56ee-470f-b81e-e5a367a65bfb

We noticed a discrepancy between the information presented in the website, and that found in the downloaded csv. 
As a case study, we picked an animal "Pseudaspis cana" and checked the information on the website here: https://api.eol.org/pages/962419
this shows traits like "Body symmetry", "are eaten by" etc. 

In order to check if this information is available locally, we first try to find all traits for this animal. We did this by

```
DFtraits[DFtraits['scientific_name'] == "Pseudaspis cana"]
```

In order to find out if the trait is present, we first checked if there's a mapping from trait name to trait_id or some other representation, like so:

``` 
$ grep -E 'are eaten by' Data/traits/terms.csv
http://purl.obolibrary.org/obo/RO_0002471,are eaten by,association
```

Next, we filtered by checking for the presence of these traits as string values. 

```
# DFtraits[DFtraits['page_id'] == 962419].apply(lambda row: row.astype(str).str.contains('RO_0002471').any(), axis=1)
```

The above search yield false for all values, indicating that no such trait was present.
We repeated this for another animal 'Serpentes' and did find the presense of one trait (which validated the methodology), but confirmed the hypothesis that the data is incomplete for many animals.