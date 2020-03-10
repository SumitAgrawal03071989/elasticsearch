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

## Normalization

Tokenization enables matching on individual terms, but each token is still matched literally. This means:

- A search for Quick would not match quick, even though you likely want either term to match the other
- Although fox and foxes share the same root word, a search for foxes would not match fox or vice versa
- A search for jumps would not match leaps. While they don’t share a root word, they are synonyms and have a similar meaning.

To solve these problems, text analysis can normalize these tokens into a standard format. This allows you to match tokens that are not exactly the same as the search terms, but similar enough to still be relevant. For example:

- Quick can be lowercased: quick.
- foxes can be stemmed, or reduced to its root word: fox.
- jump and leap are synonyms and can be indexed as a single word: jump.

To ensure search terms match these words as intended, you can apply the same tokenization and normalization rules to the query string. For example, a search for Foxes leap can be normalized to a search for fox jump.

# Concepts

## Anatomy of an analyzer

An analyzer  — whether built-in or custom — is just a package which contains three lower-level building blocks: character filters, tokenizers, and token filters.

1. Character filters
  A character filter receives the original text as a stream of characters and can transform the stream by adding, removing, or changing characters. For instance, a character filter could be used to convert Hindu-Arabic numerals (٠‎١٢٣٤٥٦٧٨‎٩‎) into their Arabic-Latin equivalents (0123456789), or to strip HTML elements like "<b>" from the stream.
2. Tokenizer  
  A tokenizer receives a stream of characters, breaks it up into individual tokens (usually individual words), and outputs a stream of tokens.
3. Token filters
  A token filter receives the token stream and may add, remove, or change tokens. For example, a lowercase token filter converts all tokens to lowercase, a stop token filter removes common words (stop words) like the from the token stream, and a synonym token filter introduces synonyms into the token stream.

## Index and search analysis

Text analysis occurs at two times:

1. Index time
  When a document is indexed, any text field values are analyzed.
2. Search time 
  When running a full-text search on a text field, the query string (the text the user is searching for) is analyzed. Search time is also called query time.









