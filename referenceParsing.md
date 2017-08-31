# Reference Parsing

One of the most fundamental challenges for any journal lies in dealing with references, especially if you intend to deliver them in a machine readable format such as [JATS](https://jats.nlm.nih.gov).

There are many reasons to convert your references to a machine-readable format – not in the least of which is that it can mean a more reader-friendly experience for your human readers.

## Automated reference parsing with Anystyle

If you have unstructured, plain-text references, a good tool to use is the [anystyle.io](http://anystyle.io) web app, which uses machine learning algorithms (based on conditional random field) to parse references.

### Preparation of input data

Anystyle is not a magic bullet. In all likelihood, you will have to do some manual correction to the reference lists in the token editor.  The reference lists you give it should be:

- **Complete**. Ideally, each reference in the reference list should have all pieces of information (e.g. publisher, editors, doi) contained within it.
- **Consistent**, preferably according to a known standard. This is especially important when you interact with the references in the token editor, since you will need to apply 'tokens' to the parts of each reference in reproducible manner.
- **Free of typos**. In particular, free of e.g. missing spaces or misplaced punctuation. Anystyle uses these marks to distinguish one part of a citation from another.

### Tag clarifications

Anystyle uses its **token editor** to match parts of your reference lists to the structured data that it outputs. It is important to note that *these _tokens_ do not map 1:1 onto the outputs*. That is to say, do not consider the token presented in the anystyle editor in terms of either the a) outputs that you will get from that app or b) the scholarly reference terms you are acquainted with. In order to learn the ways in which the app processes data, it is best to parse some reference lists and compare the results with the original documents before proceeding.

### Export

Upon completion, Anystyle will give you a few formats to export your references to: **citeproc/json**, **xml**, and **bibtex**. While all of these have their uses, citeproc/json is the most versatile and complete format. Choose this option.

## Using the structured data

Implementing the structured data in your journal depends on what kind of ouput formats you intend to use. It is useful to consider, for instance, if you want to:
- deliver **machine-readable HTML**. This means that the website itself will contain the structured reference data, albeit most likely not in a format readable to humans. Options for storing machine-readable reference data include JSON-LD and RDF.


- 

In most cases, you want to 

### JATS

### Microdata

- JSON-LD
