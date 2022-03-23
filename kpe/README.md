# Keyphrase Extraction
All datasets in this directory are datasets for the problem of Keyphrase Extraction (see [this sibling repository](https://github.com/zxul767/gist/tree/master/research/kpe) for full details on the problem.)

# Motivation
Even though there are a few repositories[^1] that have compiled the most famous datasets referenced in the literature, we found that almost all of them were incomplete in one sense or another. In many cases, information was lacking about annotators of the gold keyphrases (i.e., authors, readers, professional indexers); in other cases, information was lacking about a partitioning used by the authors into `training`, `validation` and `test` subsets.

This made it harder to explain the discrepancies between the published literature and our own results while reproducing various papers. As a way to mitigate this (and also to avoid dependence on repositories which we do not control), we decided to build this repository.

# Organization
Each dataset is available under its own directory, as a compressed `<dataset-id>.tar.gz` file. Upon decompression, you should find three folders (`training`, `validation`, `test`) and one `metadata.json` file:

```
├── [zxul767   851]  metadata.json
├── [zxul767   36K]  test
|───── [zxul767   1.1K]  <doc-id>.keys.json
|───── [zxul767   1.2K]  <doc-id>.txt
├── [zxul767   68K]  training
└── [zxul767   36K]  validation
```

## Partitions
The `training` folder is guaranteed to always exist, but the presence of the `validation` and `test` folders depends on whether the original dataset authors published their dataset partitioned (this is usually documented in the accompannying paper.)

## Dataset Pre-Processing
As can be seen above, each of the folders contains files of the form `<doc-id>.keys.json` (gold keyphrases), and `<doc-id>.txt` (document's text). 

`<doc-id>.txt` is the result of removing redundant whitespace (i.e., newlines, spaces and tabs) from the original document. More specifically, we remove trailing and leading whitespace, and collapse contiguous chunks of whitespace in other parts of the document (e.g., the sequence `\n\n\t\t ` would be turned into `\n\t `).

`<doc-id>.keys.json` has the following structure:
```json
  {
   "literal": {
      "authors": [],
      "readers": [],
      "indexers": [
         "wavelength services"
      ]
   },
   "all": {
      "authors": [],
      "readers": [],
      "indexers": [
         "PointEast Research",
         "telecommunication",
         "optical fibre networks",
         "Looking Glass Networks",
         "wavelength services",
         "fiber optic networks"
      ]
   }
}
```

### Literal Keyphrases
Given that the problem at hand is keyphrase extraction--and not keyphrases or topic inference--it's important to distinguish between phrases that appear directly in the text, and those that were inferred by annotators. For almost every dataset published for this problem, this distinction is not made explicit.

The key `literal` groups all gold keyphrases that show up almost literally in the document (i.e., a case-insensitive search spanning multiple lines would find the phrase in the document). Beware that this definition of "literal" means that `game`, `gaming`, and `games` would be considered different.

Due to the imperfect format conversion done by the authors, some documents (which were originally PDF files) have text that would naturally be a single hard line, partitioned into multiple hard lines. This requires special attention when looking for literal occurrences of keyphrases (e.g., a simply regex search for `one term` might not find an occurrence if the phrase appears literally as `one\n\n\tterm` in the text.)

### Gold Keyphrases Annotators
Each of the keys `authors`, `readers` and `indexers` corresponds to keyphrases that were originally annotated by either the documents' authors, by an assigned group of readers, or by a set of professional indexers, respectively.

Having information about these sources is useful to see the contrast in the results of various algorithms depending on the evaluation criteria adopted (e.g., `authors` might be more likely to be concise in the keywords they annotate, whereas `readers` could be more liberal)

[^1]: See https://github.com/snkim/AutomaticKeyphraseExtraction/, https://github.com/LIAAD/KeywordExtractor-Datasets/
