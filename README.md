# Desciptive analysis from The Pangloss Collection data 

The aim of this project was to do a descriptive analysis of the linguistic data acquired in the [Pangloss Collection](https://pangloss.cnrs.fr/index_en.htm "PANGLOSS website"), to be able to create a tool that would group all glosses by type.

The Pangloss Collection is a database of media in under-documented languages. One type of data Pangloss has are transcripted texts, with their gloses (per word, per morphem, or both) and with a translation in another language (English, French...). The "problem" with this kind of data, though, is that because different researches do the work, it is difficult to have unified gloses and translation. The ultimate goal of the project I have been working on, it to create a tool that would permit unify all those gloses in order for linguists to compare different languages. This bunch of code is the beginning of this project: descriptive analysis of the dataset to see how does it look like, before gettnig into the creation of the tool itself.

#### Table of Contents

* [Grabbing the Data](#data)
* [First Parsing](#parse)
* [Glosses descriptive analysis](#gloss)
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
Then, I proceeded to look more into the gloses themselves, grabbing the following informations for each language:
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
Then, I applied this function to **"glose_word"** and to **"glose_morphem"** and saved the data in another file (*gloss.pkl*), so it would be easier to treat afterwards:
```python
import pickle

used_gloss_morphem= used_glosses("glose_morphem", data)
used_gloss_word= used_glosses("glose_word", data)
pickle.dump((used_gloss_word, used_gloss_morphem), open("gloss.pkl", "wb"))
```
## <a name="gloss"></a>Glosses descriptive analysis
Once I had the new file, created a new function that would convert my .pkl to a list of all the present gloses in the dataset and BLAHBLAH
```python
def from_data_to_list(pickledata, num):
    data = pickledata[num]
    del(data[None])
    biglist = []
    for key in data:
        list=[key, len(data[key])]
        biglist.append(list)
    sorted_biglist = sorted(biglist, key=itemgetter(1), reverse = True)
    return(sorted_biglist)
```
To have an overall look of the data in glosses we had and their distribution, I created a function to make and save graphs of:
* The first 10 gloses
* The middle 10 gloses
* The last 10 gloses
* The first 100 gloses
* The middle 100 gloses
* The last 100 gloses
```python
import pickle
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.font_manager import FontProperties
from operator import itemgetter
from collections import defaultdict

def make_graphs(pickledata, num,type):
    sorted_biglist = from_data_to_list(pickledata,num)

    #First 10
    first10 = sorted_biglist[:10]
    labels, ys = zip(*first10)
    plt.figure(figsize=(20, 15))  # width:10, height:8
    xs = np.arange(len(labels))
    width = 1
    plt.bar(xs, ys, width, align='center')
    plt.xticks(xs, labels, rotation=90, FontProperties=ChineseFont) #Replace default x-ticks with xs, then replace xs with labels
    plt.savefig('first_10_{}.png'.format(type))

    #Middle 10
    num = int(len(sorted_biglist)/2)
    middle10 = sorted_biglist[num-5:num+5]
    labels, ys = zip(*middle10)
    plt.figure(figsize=(20, 15))  # width:10, height:8
    xs = np.arange(len(labels))
    width = 1
    plt.bar(xs, ys, width, align='center')
    plt.xticks(xs, labels, rotation=90, FontProperties=ChineseFont) #Replace default x-ticks with xs, then replace xs with labels
    plt.savefig('middle_10_{}.png'.format(type))

    #Last 10
    last10 = sorted_biglist[-10:]
    labels, ys = zip(*last10)
    plt.figure(figsize=(20, 15))  # width:10, height:8
    xs = np.arange(len(labels))
    width = 1
    plt.bar(xs, ys, width, align='center')
    plt.xticks(xs, labels, rotation=90, FontProperties=ChineseFont) #Replace default x-ticks with xs, then replace xs with labels
    plt.savefig('last_10_{}.png'.format(type))

    #First 100
    first100 = sorted_biglist[:100]
    labels, ys = zip(*first100)
    plt.figure(figsize=(20, 15))  # width:10, height:8
    xs = np.arange(len(labels))
    width = 1
    plt.bar(xs, ys, width, align='center')
    plt.xticks(xs, labels, rotation=90, FontProperties=ChineseFont) #Replace default x-ticks with xs, then replace xs with labels
    plt.savefig('first_100_{}.png'.format(type))

    #Middle 100
    num = int(len(sorted_biglist)/2)
    middle100 = sorted_biglist[num-50:num+50]
    labels, ys = zip(*middle100)
    plt.figure(figsize=(20, 15))  # width:10, height:8
    xs = np.arange(len(labels))
    width = 1
    plt.bar(xs, ys, width, align='center')
    plt.xticks(xs, labels, rotation=90, FontProperties=ChineseFont) #Replace default x-ticks with xs, then replace xs with labels
    plt.savefig('middle_100_{}.png'.format(type))

    #Last 100
    last10 = sorted_biglist[-100:]
    labels, ys = zip(*last10)
    plt.figure(figsize=(20, 15))  # width:10, height:8
    xs = np.arange(len(labels))
    width = 1
    rwidth = 0.5
    plt.bar(xs, ys, width, align='center')
    plt.xticks(xs, labels, rotation=90, FontProperties=ChineseFont) #Replace default x-ticks with xs, then replace xs with labels
    plt.savefig('last_100_{}.png'.format(type))
```
In order to run this function, I wrote the following code:
```python
#Graph for words:
#make_graphs(pickledata,0,"words")

#Graph for morphemes:
#make_graphs(pickledata,1,"morphemes")
```

Finally, I decided also to look 

```python
#Graph for words:
#make_graphs(pickledata,0,"words")

#Graph for morphemes:
#make_graphs(pickledata,1,"morphemes")
```
