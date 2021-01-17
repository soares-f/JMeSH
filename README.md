# JMeSH - Japanese MeSH Silver Corpus for COVID-19 related literature

This project, for the Biomedical Linked Annotation Hackathon 7 (BLAH7) aims at providing a Japanese controlled vocabulary of medical terms related to COVID-19.  
From previous editions of BLAH, such as BLAH 6, and BLAH5, there were efforts on providing resources for an open Japanese MeSH-like controlled vocabulary. However, those were open domain.

## Goal
 - To create a controlled vocabulary in Japanese that can be mapped to English MeSH
 - To devise a system that can be language agnostic for the creation and mapping of multilingual controlled vocabularies to MeSH
 
## Approach

Previous approaches to create a linkeable controlled vocabulary for Japanese have resorted to existing bilingual dictionary and transformation rules to allow such mappings. However, given the possible new terms introduced due to COVID-19 and the emphasis on respiratory and infection related terms, we want to take advantage of the already mapped terms in English and the use of automatic Translation.  

A straightforward approach would be to directly translate the terms from English to Japanese using commercially available translators, such as Google Translate or Bing. However, given past experiments, this course of action usually results in ill-translated terms. One of the reasons for such behavior is that modern machine translation systems take huge advantage of context in a sentence to make the translation of a single token.  

Thus, in our approach, we will make use of full sentences in English that contain an specific desired MeSH term. After translation to Japanese, the equivalent in the target language will be found. This task can be described as the construction of bilingual dictionaries from parallel data (e.g. https://strathprints.strath.ac.uk/2464/6/strathprints002464.pdf).  

However, since translating just one sentence for each term might lead to noisy results, we will look for a larget set of sentences containing the same English MeSH term and then translate them to Japanese. Using corpus linguistics tools, such as cross-language co-occurence analysis coupled with Expected Mutual Information Measure (EMIM).

## Pipeline

1. Gathering all COVID articles with an assigned MeSH  

2. For all MeSH in 1, check if there is already an equivalent in previous open Japanese MeSH
  - For example, it may be possible that coagulopathy, pulmonary hypertension, and some cytokines are not already translated, so they would be good candidates.
3. From the candidates, retrieve from the already tagged LitCovid corpus those entities (MeSH concepts) (with sentences in English).
  - In a covid text: "Pulmonary embolism has a high prevalence in COVID-19 patients."
  - Pulmonary embolism would have a MeSH, then we could get other sentences that have Pulmonary embolism, like "Pulmonary embolism is shown to increase the risk of death. The patient presented bilateral Pulmonary embolism" and so on.
4. Use an MT system to get the translation from English to Japanese (for whole sentences to make use of full context)
From the examples above:

English | MT Japanese
------------- | -------------
**Pulmonary embolism** has a high prevalence in COVID-19 patients  | **肺塞栓症**はCOVID-19患者で高い有病率を示す
**Pulmonary embolism** is shown to increase the risk of death | **肺塞栓症**は死亡リスクを高めることが示されている
The patient presented bilateral **Pulmonary embolism** | 症例は両側性**肺塞栓症**を呈した。

5. Using a frequency approach or other corpus linguistics approach, find possible Japanese equivalents.
  - In the table above, it seems that **肺塞栓症** is a good candidate for pulmonary embolism.  
Those Japanese equivalents could be a "starting point" for further research

## Resources

 - LitCovid (https://www.ncbi.nlm.nih.gov/research/pubtator/index.html?view=docsum&query=$LitCovid)
 - AWS Translate API (https://aws.amazon.com/translate/)
 - DeepL Pro API (https://www.deepl.com/translator)
