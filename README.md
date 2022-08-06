# NameGame

Analyze the gender distribution of children's book writers and use sound to match names to gender.

## Project Description

The same name can be spelled out in a many ways (for example, Marc and Mark, or Elizabeth and Elisabeth). Sound can, therefore, be a better way to match names than spelling. In this project, you will use the Python package Fuzzy to find out the genders of authors that have appeared in the New York Times Best Seller list for Children's Picture books.

First, using fuzzy (sound) name matching, you will search for author names in a dataset provided by the US Social Security Administration that contains names and genders of all individuals who have applied for Social Security Cards. Next, we'll aggregate the author dataset by including gender. Finally, you will use the new dataset to plot the gender distribution of children's picture books authors over time.

To complete this project, you should be familiar with pandas DataFrames, NumPy for basic statistics, and Matplotlib for plotting.

## 1. Sound it out!

Grey and Gray. Colour and Color. Words like these have been the cause of many heated arguments between Brits and Americans. Accents (and jokes) aside, there are many words that are pronounced the same way but have different spellings. While it is easy for us to realize their equivalence, basic programming commands will fail to equate such two strings.

More extreme than word spellings are names because people have more flexibility in choosing to spell a name in a certain way. To some extent, tradition sometimes governs the way a name is spelled, which limits the number of variations of any given English name. But if we consider global names and their associated English spellings, you can only imagine how many ways they can be spelled out.

One way to tackle this challenge is to write a program that checks if two strings sound the same, instead of checking for equivalence in spellings. We'll do that here using fuzzy name matching.

```
# Importing the fuzzy package
import fuzzy

# Exploring the output of fuzzy.nysiis
fuzzy.nysiis('Grey')

# Testing equivalence of similar sounding words
fuzzy.nysiis('Gray')

```

## 2. Authoring the authors

The New York Times puts out a weekly list of best-selling books from different genres, and which has been published since the 1930’s. We’ll focus on Children’s Picture Books, and analyze the gender distribution of authors to see if there have been changes over time. We'll begin by reading in the data on the best selling authors from 2008 to 2017.

```
# Importing the pandas module
import pandas as pd

# Reading in datasets/nytkids_yearly.csv, which is semicolon delimited.
author_df = pd.read_csv("datasets/nytkids_yearly.csv", sep= ";")

# Looping through author_df['Author'] to extract the authors first names
first_name = []
for name in author_df['Author']:
    first_name.append(name.split()[0])

# Adding first_name as a column to author_df
author_df['first_name'] = first_name

# Checking out the first few rows of author_df
author_df.head()
```
![image](https://user-images.githubusercontent.com/84130785/183251168-d393e587-ae3e-41c8-a188-850a9debbf0d.png)

## 3. It's time to bring on the phonics... again!

When we were young children, we were taught to read using phonics; sounding out the letters that compose words. So let's relive history and do that again, but using python this time. We will now create a new column or list that contains the phonetic equivalent of every first name that we just extracted.

To make sure we're on the right track, let's compare the number of unique values in the first_name column and the number of unique values in the nysiis coded column. As a rule of thumb, the number of unique nysiis first names should be less than or equal to the number of actual first names.

```
# Importing numpy
import numpy as np

# Looping through author's first names to create the nysiis (fuzzy) equivalent
nysiis_name = []
for firstname in author_df['first_name']:
    nysiis_name.append(fuzzy.nysiis(firstname))

# Adding nysiis_name as a column to author_df
author_df['nysiis_name'] = nysiis_name

# Printing out the difference between unique firstnames and unique nysiis_names:
diff_names = len(np.unique(author_df.first_name)) - \
    len(np.unique(author_df.nysiis_name))
print('There are ' + str(diff_names) +
      ' more unique values for first_name than nysiis_name')
```
![image](https://user-images.githubusercontent.com/84130785/183251189-15143189-1a5d-4009-91d5-e1af81e299fe.png)

## 4. The inbetweeners

We'll use babynames_nysiis.csv, a dataset that is derived from the Social Security Administration’s baby name data, to identify author genders. The dataset contains unique NYSIIS versions of baby names, and also includes the percentage of times the name appeared as a female name (perc_female) and the percentage of times it appeared as a male name (perc_male).

We'll use this data to create a list of gender. Let's make the following simplifying assumption: For each name, if perc_female is greater than perc_male then assume the name is female, if perc_female is less than perc_male then assume it is a male name, and if the percentages are equal then it's a "neutral" name.












