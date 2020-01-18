# Desciptive analysis from PANGLOSS data 

The aim of this project was to do a descriptive analysis of the linguistic data acquired in the [Pangloss Collection](https://pangloss.cnrs.fr/index_en.htm "PANGLOSS website"), to be able to create a tool that would group all glosses by type.

The Pangloss Collection is a database of media in under-documented languages. One type of data Pangloss has are transcripted texts, with their gloses (per word, per morphem, or both) and with a translation in another language (English, French...). The "problem" with this kind of data, though, is that because different researches do the work, it is difficult to have unified gloses and translation. The ultimate goal of the project I have been working on, it to create a tool that would permit unify all those gloses in order for linguists to compare different languages. This bunch of code is the beginning of this project: descriptive analysis of the dataset to see how does it look like, before gettnig into the creation of the tool itself.

## Table of Contents

* [Grabbing the Data](#data)
* [First Parsing](#parse)
* [Bibliography](#bibliography)
* [What I learned from this course](#learned)

## <a name="data"></a>Grabbing the data
The data of The Pangloss Collection has been extracted from diverse XML files and recorded in a JSON (*pangloss.json*) in the form of a list of dictionnaries as the following:
```python
{"lang": "tvk", "src": "Napol goni transleta tei .", "en": "I work as a translator", "bi": "mi wok olsem wan translator", "tokenized_src_morphem": ["na", "pol", "goni", "transleta", "tei"], "glose_morphem": ["1s.nfut", "work", "3s.be_like.ind", "translator", "one"]}
```
Where: 
* **"lang"** represents the code of the sampled langue
* **"src"**, the transcripted sentence
* **"en"**/**"bi"**/**"fr"**..., the languages in which the source has been translaetd in
* **"tokenized_src_morphem"**, the token morphems present in the source
* **"glose_morphem"**, the glose for each one of this morphems
* **"tokenized_src_word"**, the token words present in the source *(Not present in this example)*
* **"glose_word"**, the glose for each one of this words *(Not present in this example)*

This file has been the one used for the ensuing analysis, importing it with the following code:
```python
import json
import pandas as pd

with open('pangloss.json', 'r') as json_file:
    jsondata = json.load(json_file)[0]

data = pd.DataFrame(jsondata)
```

## <a name="parse"></a>First Parsing
For the first steps of the parsing, I wanted to have a general view of the DataFrame, know how many phrases we had in total, and finally which languages and how many were we dealing with. To do so, I used the following code (and got the following output):
```python
# general view of the DataFrame
df_general = data.count()

# nb of phrases in total
nb_phrases= data.shape[0]

# nb lang
nb_lang = data["lang"].nunique()

# list of languages
list_name_lang = data["lang"].unique() 
```



to grab the following informations for each language:
* Its gloses-per-word
* Its gloses-per-morphem
* The number of times each glose has been used in each language

To do so, I created the following function:
```python
import numpy as np
from collections import defaultdict

def used_glosses(gloses, data):
    used_gloss = defaultdict(list)
    for _, row in data.iterrows():
        if row[gloses] is not np.nan:
            for glose_list in list(row[morpheme]):
                print(glose_list)
                used_gloss[glose_list].append(row["lang"])
    return(used_gloss)
```
Then, I applied this function

My project will be about linguistic data. From a json, parsing the data we are interested in and performing informational statistics + creating new documents to store this data.

Linguistic data is of the following form: target-language-sentences, traduction-into-other-languages, glose-by-morpheme, glose-by-word, parsed-morphemes, parsed-words.

#Still have to concrete with my internship tutor.
