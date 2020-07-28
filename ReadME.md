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