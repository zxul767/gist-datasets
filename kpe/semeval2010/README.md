# Contents

+ `semeval2010.tar.gz` is the dataset in our own format. 
+ `sources/semeval2010-original-partitioned.tar.gz` is likely the original version from the [SemEval2010](https://semeval.github.io/) contest, originally obtained from https://github.com/snkim/AutomaticKeyphraseExtraction
+ `sources/semeval2010-abstracts.tar.gz` is another version found in https://github.com/LIAAD/KeywordExtractor-Datasets which contains only abstracts of the documents.

The reason why we needed to source two versions of the same dataset is that, while `semeval2010-original-partitioned.tar.gz` preserved important information regarding the source of the gold keyphrases (i.e., whether they were annotated by authors or readers), the test partition didn't contain files for unstemmed keyphrases (as the training and validation partitions did). We suspect this is probably due to the logistics of the contest, which required that students didn't get access to the gold keyphrases in the test partition (and they were later added, but only in their stemmed version.)

Because of the above, we had to find a way to reconstruct the unstemmed keyphrases for the test partition. Part of the code we used to perform that reconstruction can be found [here](https://gist.github.com/zxul767/b69fc59ca27a03b96677de33f0a64f10)[^1] 

For details on our version, see the README in the parent folder.

[^1]: We intend to add the full scripts for fully recreating `semeval2010.tar.gz` from its sources later on; see [this issue](https://github.com/zxul767/gist-datasets/issues/1). 
