# Desciptive analysis from PANGLOSS data 

The aim of this project was to do a descriptive analysis of the linguistic data acquired in the [Pangloss Collection](https://pangloss.cnrs.fr/index_en.htm "PANGLOSS website"), to be able to create a tool that would group all glosses by type.

The Pangloss Collection is a database of media in under-documented languages. One type of data Pangloss has are transcripted texts, with their gloses (per word, per morphem, or both) and with a translation in another language (English, French...). The "problem" with this kind of data, though, is that because different researches do the work, it is difficult to have unified gloses and translation. The ultimate goal of the project I have been working on, it to create a tool that would permit unify all those gloses in order for linguists to compare different languages. This bunch of code is the beginning of this project: descriptive analysis of the dataset to see how does it look like, before gettnig into the creation of the tool itself.

## Table of Contents

## Getting to know the dataset, the big picture.

The ressorces I used for these descriptive analysis were, for each language:
* Its gloses-per-word
* Its words
* Its gloses-per-morphem
* Its morphems
* The languages each language has been translated in

My project will be about linguistic data. From a json, parsing the data we are interested in and performing informational statistics + creating new documents to store this data.

Linguistic data is of the following form: target-language-sentences, traduction-into-other-languages, glose-by-morpheme, glose-by-word, parsed-morphemes, parsed-words.

#Still have to concrete with my internship tutor.
