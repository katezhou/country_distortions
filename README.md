# Richer Countries and Richer Representations

# GDP_distortions
This repository is meant to help recreate the results from the paper: Richer Countries and Richer Representations (https://arxiv.org/abs/2205.05093).

The core analysis for the two papers are in ```Section_3_4/GDP Distortions Analysis.ipynb``` and ```Section_5/fill_mask_task_analysis.ipynb```. In order to reproduce the results, there are five main steps: downloading various data sources (data on countries), calculating word frequencies from Wikipedia and BookCorpus, building contexutalized embeddings, performing analysis on these embeddings, and performing analysis on a fill mask task.


In more detail:

1. **Calculate word frequencies for Wikipedia and BookCorpus as a way to estimate the word distribution of the training corpus of BERT**
Calculate word frequencies for Wikipedia and BookCorpus We used tools such as wikiextractor and wiki-word-frequencies to help calculate the word frequencies of Wikipedia corpus we have. We then used word frequencies from Complex Word Identification (CWI) Shared Task 2018 project as a way to estimate BookCorpus word frequencies(https://github.com/nathanshartmann/NILC-at-CWI-2018). Country name frequencies can be found: ```country_counts_bookcorpus.csv``` and ```country_counts_wiki.csv```. See: ```Section_3_4/counting_country_frequencies/counting_multiword_country_counts.ipynb```

2. **Gather meta data information about countries (names, and GDP) and combine with frequency data**
Preprocess the metadata associated with the countries: ```preprocess_countries_metadata.ipynb```

3. **Query the Wikipedia corpus for names of countries and create contextualized word embeddings names of words**
Gather a large corpus of text to create contextualized word embeddings Download a large corpus of text which will be used to create contextualized word embeddings. We used the March 1, 2020 Wikimedia Download which can be found here: https://dumps.wikimedia.org/. We later want to be able to query this corpus for examples of words in context. We scramble this Wikipedia corpus (at the sentence level) so that we can query the first 50 examples found and avoid biases certain Wikipedia pages/topics. See file: "country_sentences.csv" which contains sentences from Wikipedia that contains names of countries. 

4. **Create contextualized word embeddings sentences from Wikipedia corpus "creating_word_embedding.ipynb".**
Create contextualized word embeddings for names of countries. This involves embedding the entire sentence and then extract the word embedding associated with the word of interest. We then used Hugging Face's library to create contexutalized word embeddings. See file: ```Section_3_4/embeddings/creating_word_embedding.ipynb```. The resulting files should be titled ```Section_3_4/embeddings/country_names_average_embeddings.csv``` and ```Section_3_4/embeddings/country_names_first_embeddings.csv```. For the analysis, we also create embeddings of names of countries in "artificial contexts" to ensure that the words appear in the same context. The sentences used can be found here: ```Section_3_4/artificial_context/artifical_data.csv```. Replace the token with the names of countries and created embeddings the same way.

5. **Perform analysis: tokenize the names of countries and compare cosine similarity results of names of countries with other names**
Run the file: ```Section_3_4/GDP Distortions Analysis.ipynb```. The results for the cosine analysis (intermediate results) are in ```Section_3_4/cosine_results/```. 

6. **Measure the accuracy of a BERT in a simple masked language modeling task.**
The purpose of this task is to see how BERT will perform on a simple masked language modeling task where we mask out the name of a country.  First, you will want to gather data ready for the fill mask task. We take sentences from Wikipedia that contains the name of a country (and remove sentences with multiple names of countries.) These sentences can be found: ```pre_training_country_text.txt``` and the cleaned version is called ```pre_training_country_text.csv``` (both are zipped for sharing).  Then use ```Section_5/creating_fine_tuning_training_data.ipynb``` to create fine-tuning data files: ```fine_tuning_test_data.csv``` and "```fine_tuning_training_data.csv```. Lastly, use ```Section_5/fill_mask_task_analysis.ipynb``` for the analysis of this task. Prediction results (intermediate results) can be found in: ```Section_5/model_prediction_results/```

7. **Fine-tune models on corpus focused on 20 randomly selected countries and measure how accuracy changes**
```Section_5/fine_tuning_bert_model.ipynb```



