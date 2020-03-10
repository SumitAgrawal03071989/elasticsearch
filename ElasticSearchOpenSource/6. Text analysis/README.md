# Text analysis

Text analysis is the process of converting unstructured text, like the body of an email or a product description, into a structured format that’s optimized for search.

## When to configure text analysis

Elasticsearch performs text analysis when indexing or searching text fields.

If you use text fields or your text searches aren’t returning results as expected, configuring text analysis can often help. You should also look into analysis configuration if you’re using Elasticsearch to:

- Build a search engine
- Mine unstructured data
- Fine-tune search for a specific language
- Perform lexicographic or linguistic research


## Text analysis overview

Text analysis enables Elasticsearch to perform full-text search, where the search returns all relevant results rather than just exact matches.

If you search for **Quick fox jumps**, you probably want the document that contains **A quick brown fox jumps over the lazy dog**, and you might also want documents that contain related words like **fast fox** or **foxes leap**.


## Tokenization 

Analysis makes full-text search possible through tokenization: breaking a text down into smaller chunks, called tokens. In most cases, these tokens are individual words.

If you index the phrase **the quick brown fox jumps** as a single string and the user searches for **quick fox**, it isn’t considered a match. However, if you tokenize the phrase and index each word separately, the terms in the query string can be looked up individually. This means they can be matched by searches for quick fox, fox brown, or other variations.

## 
