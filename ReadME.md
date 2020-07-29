# NLP - AutoCorrect
Autocorrect is a software feature that corrects misspellings as you type. It is integrated into mobile operating systems like Android and iOS and is therefore a standard feature on most smartphones and tablets. It is also supported by modern desktop operating systems, such as Windows and macOS.

Autocorrect makes it easier to type words on a mobile device with a touchscreens. Since these devices often have tiny onscreen keyboards, it is easy to make mistakes as you type. Autocorrect can fix these mistakes on the fly, helping you type faster without having to go back and correct your typos.

![AutocorrectImage](./imgs/autocorrect.png)

We will implement a simple autocorrect system using facebook messages dataand a probabilistic model.

## Progress

### Data Collection
NLP system require a large amount of text data. As Autocorrect systems are highly dependent on the users preference of words, I am going to use my facebook messages to create the system. The data is downloaded from facebook. *You can use the the pickle file to load the preprocessed dictionary and probability dictionary for test*. If you wish to use your data instead, you can download your facebook messages in **JSON** format and place the inbox folder in the root directory. 

### Data Analysis and Preprocessing
By ispecting eacg JSON file, the data is structured with main 4 key properties,
- participants
- messages
- title
- is_still_participant

I use participants property to filter out one to one conversations, excluding group chats. Inside the *messages* property we have serveral subproperties depending of different forms of messages being exchanged. We will use only 2 properties amoung all of them,
- *sender_name* : To select messages send by only one person
- *content* : The actual message sent
 
Data will be lower cased and punctuations are to be removed but words like "it's", "don't" etc are kept without any change. Will include these punctuation characters as part of the edits made to words.

### Creating Vocabulary
Once the data is preprocessed, we use a `Counter` to iterate over all the data and create a vocabulary dictionary containing words as keys and the number of time they appear in the corpus as values. Next we need to find the probabilities of occurrence of each word from the corpus. For this we use the following formula,
```math
P(W_i) = C(W) / N

where,
C(N) : Number of occurrences of word W in corpus
N : Total number of words in Corpus
```

### Define Edit Methods
Now that we have our vocab and probability dictionary, We implement the edits we will make to generate possible words from the input word. We define 4 types of edits, 
- **delete_letter**: given a word, it returns all the possible strings that have one character removed.
- **switch_letter**: given a word, it returns all the possible strings that have two adjacent letters switched.
- **replace_letter**: given a word, it returns all the possible strings that have one character replaced by another different letter.
- **insert_letter**: given a word, it returns all the possible strings that have an additional character inserted.

### Correction Algorithm
At last we define our correction algorithm as,
- If the word is in vocabulary, return the word (correct word)
    - Else get all strings that are one edit away
        - Check for strings in vocabulary
        - If found, return strings
        - Else Get all strings that are two edits away
            - Check for strings in vocabulary
            - If found, return strings
            - Else return the original string (no correcctions available)

The algorithm is an Greedy Algorithm as we are searching for strings that are at minimum edits from the input string at all time.

### Running the autocorrect
Input Code:
```python
my_word = 'hbi'
n_best = 3
vocab = list(vocab_word_count.keys())
corrections = get_corrections(my_word, prob_dict, vocab, n = n_best)

for i, word_probs in enumerate(corrections):
    print("Word : {}, probability : {:.6f}".format(word_probs[0], word_probs[1]))
```
Output:
```
Word : hai, probability : 0.012827
Word : bhi, probability : 0.003414
Word : hui, probability : 0.000785
```

# References
- **deeplearning.ai** - https://www.deeplearning.ai/natural-language-processing-specialization/