# Bio data observations

Schema.org structure analysis for several entities: https://github.com/elizusha/scraper/tree/master/data_analysis

## Proteins

### Different name of the same property

Same protein in different data sources can have the same value specified in different properties. For example property  https://schema.org/hasRepresentation in proteins form https://disprot.org has the same value as property https://schema.org/hasBioPolymerSequence in proteins from https://mobidb.org.

### Uniprot ID

A common way to identify a protein in through a Uniprot ID (https://uniprot.org is another protein database), this ID is present as a https://schema.org/sameAs property on a number of websites. It's therefore possible to join the protein data from different websites based on the associated Uniprot ID.

However, sometimes the property value containing a Uniprot ID can be in a slightly different format: in the https://disprot.org data it's https://www.uniprot.org/uniprot/P26554, in the https://mobidb.org data it's http://purl.uniprot.org/uniprot/P26554.
