# Example with Food 

Lets say we have our value set, reference set, related to food related diagnoses from NLM, supported and mainted by the Gravity project.

https://vsac.nlm.nih.gov/valueset/2.16.840.1.113762.1.4.1247.17/expansion/latest

| Code | Description | Code System | Code System Version | Code System OID | TTY  |
|---|---|---|---|---|---|
| 1155680000 | Limited access to nutrition supplies (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| 129832003 | Noncompliance with dietary regimen (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| 445281000124101 | Nutrition impaired due to limited access to healthful foods (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| 470911000124109 | Mild food insecurity on United States household food security survey module (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| 470941000124108 | Moderate food insecurity on United States household food security survey module (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| 470951000124105 | Severe food insecurity on United States household food security survey module (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| 706875005 | Insufficient food supply (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| 733423003 | Food insecurity (finding) | SNOMEDCT | 2023-03 | 2.16.840.1.113883.6.96 | FN  |
| E63.9 | Nutritional deficiency, unspecified | ICD10CM | 2024 | 2.16.840.1.113883.6.90 | PT  |
| Z59.41 | Food insecurity | ICD10CM | 2024 | 2.16.840.1.113883.6.90 | PT  |
| Z59.48 | Other specified lack of adequate food | ICD10CM | 2024 | 2.16.840.1.113883.6.90 | PT  |
| Z71.88 | Encounter for counseling for socioeconomic factors | ICD10CM | 2024 | 2.16.840.1.113883.6.90 | PT  |
| Z91.11 | Patient's noncompliance with dietary regimen | ICD10CM | 2024 | 2.16.840.1.113883.6.90 | HT  |
| Z91.110 | Patient's noncompliance with dietary regimen due to financial hardship | ICD10CM | 2024 | 2.16.840.1.113883.6.90 | PT  |
| Z91.A10 | Caregiver's noncompliance with patient's dietary regimen due to financial hardship | ICD10CM | 2024 | 2.16.840.1.113883.6.90 | PT |

And our example dataset (metadata) we are trying to check looks like this:

```json

{"accessLevel":"public","landingPage":"https://healthdata.gov/d/229f-a34m","issued":"2023-02-22","@type":"dcat:Dataset","modified":"2023-02-22","contactPoint":{"@type":"vcard:Contact","fn":"HHS Office of the Chief Data Officer","hasEmail":"mailto:no-reply@healthdata.gov"},"identifier":"https://healthdata.gov/api/views/229f-a34m","publisher":{"@type":"org:Organization","name":"healthdata.gov"},"description":"","title":"Access to Care (2023)"},{"accessLevel":"public","landingPage":"https://healthdata.gov/d/238m-ezg9","bureauCode":["009:00"],"issued":"2021-10-14","@type":"dcat:Dataset","modified":"2021-10-13","keyword":["drug rebate program"],"contactPoint":{"@type":"vcard:Contact","fn":"Medicaid.gov","hasEmail":"mailto:Medicaid.gov@cms.hhs.gov"},"publisher":{"@type":"org:Organization","name":"Centers for Medicare & Medicaid Services"},"identifier":"e28727b2-fe6b-46cb-8617-408de290200d","description":"The data below contains newly reported, active covered outpatient drugs which were reported by participating drug manufacturers since the last quarterly update of the Drug Products in the Medicaid Drug Rebate Program (MDRP) database.","title":"Product Data for Newly Reported Drugs in the Medicaid Drug Rebate Program 20210726 to 20210801","programCode":["009:000"],"distribution":[{"@type":"dcat:Distribution","downloadURL":"https://data.medicaid.gov/sites/default/files/uploaded_resources/mdrp-newly-rpt-drugs-aug-20210726-to-20210801.csv","mediaType":"text/csv"}],"license":"http://www.usa.gov/publicdomain/label/1.0/","accrualPeriodicity":"R/P10Y"}

```

The approach to identify SDoH references in the given metadata set using the reference list provided:


1. **Keyword List Preparation:**
    - Extract the relevant descriptions from the reference list that could be matched against the metadata.
    - Tokenize each description to split it into individual words.
    - Filter out common stop words using a predefined list (e.g., from the `nltk` library).
    - Calculate the Term Frequency (TF) for each keyword within the reference list.
    - If analyzing multiple reference lists or documents, calculate the Inverse Document Frequency (IDF) for each keyword.
    - Retain keywords with a TF-IDF value greater than 0.5.
    - Convert each keyword into its word embedding representation using a pre-trained model (Word2Vec, FastText, or BERT). Store these embeddings for use in the matching process.

2. **Metadata Parsing:**
    - Parse the metadata to extract relevant information like the title, description, and other relevant text.
    - Convert the extracted text into its word embedding representation using the same pre-trained model chosen in the previous step.

3. **Matching Algorithm:**
    - For each metadata entry, compute the cosine similarity between its word embeddings and the embeddings of the keywords from the reference list.
    - If the cosine similarity score surpasses a predefined threshold (this will need tuning based on your data and requirements), flag the metadata entry as relevant.
    - This approach will ensure even semantically similar terms, not present in the reference list, can be matched effectively.

4. **Evaluation and Refinement:**
    - Assess the performance of the matching algorithm, ensuring its accuracy in correctly flagging relevant metadata entries.
    - Refine the keyword list based on the algorithm's performance, adding or removing terms as necessary.
    - Continuously improve the algorithm, considering both exact keyword matches and their semantic relatedness to the content.

**Additional Notes:**

- Before implementing word embeddings, you'll need access to pre-trained models or the capability to train them on your own corpus. Libraries like Gensim provide easy access to Word2Vec and FastText, while HuggingFace's Transformers library can be used for BERT-derived embeddings.
- The threshold for cosine similarity will likely be different from the TF-IDF threshold, and you'll need to experiment to find an optimal value.
- Word embeddings, especially BERT-derived ones, can be computationally intensive. Ensure you have the necessary computational resources, especially if processing large datasets.

This approach incorporates word embeddings into the matching process, enabling more nuanced and semantically-aware matches.

By following this process, the algorithm is designed to discern relevant metadata entries more accurately, with a higher emphasis on keywords directly related to the subject at hand and a reduction in false positives due to the removal of stop words.

Let's start by implementing the approach in Python:

```python
import re
import json

import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize

from sklearn.feature_extraction.text import TfidfVectorizer


# Download stopwords if not available
nltk.download('stopwords')
nltk.download('punkt')

stop_words = set(stopwords.words('english'))

# Step 1: Keyword List Preparation
# Extract descriptions from the reference list
reference_list = [
    "Limited access to nutrition supplies (finding)",
    "Noncompliance with dietary regimen (finding)",
    "Nutrition impaired due to limited access to healthful foods (finding)",
    "Mild food insecurity on United States household food security survey module (finding)",
    "Moderate food insecurity on United States household food security survey module (finding)",
    # ... (other descriptions)
    "Caregiver's noncompliance with patient's dietary regimen due to financial hardship"
]

# Tokenizing and filtering stopwords
all_tokens = []
for description in reference_list:
    tokens = word_tokenize(description)
    filtered_tokens = [word.lower() for word in tokens if word.lower() not in stop_words]
    all_tokens.append(' '.join(filtered_tokens))

# Computing TF-IDF values
vectorizer = TfidfVectorizer()
tfidf_matrix = vectorizer.fit_transform(all_tokens)
feature_names = vectorizer.get_feature_names_out()
tfidf_values = tfidf_matrix.sum(axis=0).tolist()[0]
tfidf_dict = dict(zip(feature_names, tfidf_values))

print(tfidf_dict)  # This dictionary contains keywords with their respective TF-IDF values

# Extracting key words and terms from the descriptions
# Tokenizing and filtering stopwords
keywords = set()
for description in reference_list:
    tokens = word_tokenize(description)
    filtered_tokens = [word.lower() for word in tokens if word.lower() not in stop_words]
    keywords.update(filtered_tokens)
print(keywords)

# Step 2: Metadata Parsing
metadata = """[
    {
        "accessLevel": "public",
        "landingPage": "https://healthdata.gov/d/229f-a34m",
        "issued": "2023-02-22",
        "@type": "dcat:Dataset",
        "modified": "2023-02-22",
        "contactPoint": {
            "@type": "vcard:Contact",
            "fn": "HHS Office of the Chief Data Officer",
            "hasEmail": "mailto:no-reply@healthdata.gov"
        },
        "identifier": "https://healthdata.gov/api/views/229f-a34m",
        "publisher": {
            "@type": "org:Organization",
            "name": "healthdata.gov"
        },
        "description": "",
        "title": "Access to Care (2023)"
    },
    {
        "accessLevel": "public",
        "landingPage": "https://healthdata.gov/d/238m-ezg9",
        "bureauCode": ["009:00"],
        "issued": "2021-10-14",
        "@type": "dcat:Dataset",
        "modified": "2021-10-13",
        "keyword": ["drug rebate program"],
        "contactPoint": {
            "@type": "vcard:Contact",
            "fn": "Medicaid.gov",
            "hasEmail": "mailto:Medicaid.gov@cms.hhs.gov"
        },
        "publisher": {
            "@type": "org:Organization",
            "name": "Centers for Medicare & Medicaid Services"
        },
        "identifier": "e28727b2-fe6b-46cb-8617-408de290200d",
        "description": "The data below contains newly reported, active covered outpatient drugs which were reported by participating drug manufacturers since the last quarterly update of the Drug Products in the Medicaid Drug Rebate Program (MDRP) database.",
        "title": "Product Data for Newly Reported Drugs in the Medicaid Drug Rebate Program 20210726 to 20210801",
        "programCode": ["009:000"],
        "distribution": [{
            "@type": "dcat:Distribution",
            "downloadURL": "https://data.medicaid.gov/sites/default/files/uploaded_resources/mdrp-newly-rpt-drugs-aug-20210726-to-20210801.csv",
            "mediaType": "text/csv"
        }],
        "license": "http://www.usa.gov/publicdomain/label/1.0/",
        "accrualPeriodicity": "R/P10Y"
    }
]"""

metadata_data = json.loads(metadata)

# Step 3: Matching Algorithm
def check_keywords_in_metadata(metadata_entry, keywords):
    text_to_check = metadata_entry.get("description", "") + " " + metadata_entry.get("title", "")
    for keyword in keywords:
        if keyword in text_to_check.lower():
            return True
    return False

# Checking each metadata entry
for entry in metadata_data:
    if check_keywords_in_metadata(entry, keywords):
        print("Found a potential match in:", entry["title"])

```

In the above code, for simplicity, we're only matching the title and description fields of the metadata. But depending on the structure and content of the metadata, other fields might need to be included in the search process.