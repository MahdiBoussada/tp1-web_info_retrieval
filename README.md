# IR TP1: Document Preprocessing, Representation, and Indexing

## Repository Structure
```text
Brahim Mehdi Boussada/
├── README.md
├── requirements.txt
├── TP1_WEB_INFORMATION_RETRIEVAL_Brahim_Mehdi_Boussada.ipynb
└── results/
    ├── tokens_A.json
    ├── tokens_B.json
    ├── tokens_C.json
    ├── vocabulary_A.txt
    ├── vocabulary_B.txt
    ├── vocabulary_C.txt
    ├── binary_matrix_A.csv
    ├── binary_matrix_B.csv
    ├── binary_matrix_C.csv
    ├── tf_matrix_A.csv
    ├── tf_matrix_B.csv
    ├── tf_matrix_C.csv
    ├── inverted_index_A.json
    ├── inverted_index_B.json
    ├── inverted_index_C.json
    └── preprocessing_comparison.csv
```

## Implementation Choices
- **Preprocessing (`preprocess_document`):** Implemented using `str.lower()`, regex punctuation stripping (`re.sub(r'[^\w\s]', '', text)`), and whitespace tokenization (`split()`). NLTK is used strictly for the English stop-word list (`nltk.corpus.stopwords`) and `PorterStemmer`, while filtering is implemented manually via list comprehensions.
- **Vocabulary & Matrices:** Sorted vocabularies, binary term-document matrices, and term-frequency matrices are computed from scratch in Python. `pandas` is used only for formatting tables and writing CSV exports.
- **Positional Inverted Index & Search:** Built from scratch using `collections.defaultdict` mapping `term -> document_id -> [zero-based positions]`, paired with `search_term()` for document lookup and automated assertions against the manual D1–D3 index.

## Main Results
| Version | Vocabulary Size | Avg Tokens / Doc | Total Postings | Total Stored Positions |
| --- | --- | --- | --- | --- |
| A | 36 | 6.9 | 66 | 69 |
| B | 30 | 6.1 | 59 | 61 |
| C | 24 | 6.1 | 57 | 61 |

## Interpretation: Effect of Preprocessing on Vocabulary and Index
Each preprocessing stage changes the vocabulary and inverted index in specific ways. Lowercasing and removing punctuation unify surface variants such as `DOCUMENTS` and `documents;`, preventing duplicate vocabulary entries without discarding meaningful words. Removing stop words drops high-frequency function words (`the`, `and`, `to`, `are`, `about`, `more`), reducing average document length, total postings, and stored token positions while keeping content-bearing terms. Porter stemming delivers the steepest vocabulary contraction by collapsing morphological variants such as `document`/`documents`, `index`/`indexed`/`indexing`, and `retrieve`/`retrieves`/`retrieval` into single stems, which improves recall and further shrinks posting lists. However, preprocessing involves clear trade-offs: stemming can conflate distinct terms and lower precision, while deleting stop words shifts token positions and can disrupt exact phrase matching.
