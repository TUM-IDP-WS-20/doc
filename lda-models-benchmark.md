# LDA models benchmark
We showed the result of different combination of data preprocessing steps here: <a href="https://tum-idp-ws-20.github.io/doc/milestone-1-datapreprocessing-steps-comparison-based-on-LDA">Data preprocessing steps comparison based on LDA Recommendation</a>. We extend our LDA model with different configuration e.g. # of topic, # of words in each topic. In this document, we will talk about the result we got.

We tested many models but we will put here most promising 6 models in addition to previous models. The setting is the same with the previous one, we trained the model for the first 3 pages of the documents and the 10 pages of the documents. For each model, you can see the actual coding process in the notebook by clicking the model name.

Accuracy Result for the first 3-Paged Content: <br />
<iframe width="1418" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/e/2PACX-1vR0rPzUe7zfBQuYbePWyJKLhTMvT-H1YflTGvATG4a8PiNISrN8ax_ULzY9QJRG0dyJcAR9GtEXXKu8/pubchart?oid=1926428683&amp;format=interactive"></iframe>

Accuracy Result for the first 10-Paged Content: <br />
<iframe width="1413" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/e/2PACX-1vR0rPzUe7zfBQuYbePWyJKLhTMvT-H1YflTGvATG4a8PiNISrN8ax_ULzY9QJRG0dyJcAR9GtEXXKu8/pubchart?oid=713988106&amp;format=interactive"></iframe>

You can see the exact accuracy value <a href="https://docs.google.com/spreadsheets/d/1scM6RqGp_qVxDsGLxMZr-M-3TdYnD19_2Ij4uN0f7XQ/edit#gid=1784565335">here</a>.

Since removing non-English sentences from the documents provide dramatically accuracy improvement, we used the following base setting for the models 6 to 13:
- Cleaned
- Remove non-English sentences
- Remove English Stopwords(inc. MALLET)
- Lemmatized

Models: (check from <a href="https://github.com/TUM-IDP-WS-20/nlp-examples/tree/master/results">here</a>)
- Model 6# Cleaned -> Remove French Sentences -> Remove English Stopwords(inc. MALLET) -> Lemmatized
- Model 7# model_8_content_3_topic_50
- Model 8# model_8_content_3_topic_70
- Model 9# model_9_NMF_content_3_topic_30
- Model 10# model_8_replaced_content_3_topic_30
- Model 11# model_9_NMF_content_3_topic_50
- Model 12# model_8_normalized_replaced_content_3_topic_30
- Model 13# model_9_NMF_content_3_topic_70

