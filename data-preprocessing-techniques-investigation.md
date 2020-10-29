# Data Pre-processing Techniques

**Data preprocessing** is a step in machine learning to prepare the raw data to make it suitable for building and training Machine Learning models. For example, in our case, raw data is the given thousands of PDF files on Accounting literature. Since we cannot use PDF files directly in well-known machine learning/NLP models, we need to organize those files to be used in standard tools. Therefore, we data pre-processing steps will be like below for us:
- PDF text extraction
- Cleaning extracted text
   - Convert text to string
   - Remove links
   - Remove non-alphanumerics
   - Remove punctuation and lowercase
   - Remove text in square brackets
   - Remove words containing numbers
   - Remove newline characters
   - Remove unrecognized characters
- Lemmatization
- Finding and removing Stopwords
- Stemming

Each of the data preprocessing steps should be discussed before applying them to a task. The consequences of those steps might vary from task to task.
