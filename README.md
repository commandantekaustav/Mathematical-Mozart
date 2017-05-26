# Mathematical Mozart
#### Basic Idea
Mathematical Mozart basically aims at generating music without any knowledge of music itself. It can be used to generate sample music files and then use them for different needs and purposes.
A coherent, good sounding music of specified instrument is generated by this application. Music is generated by character level modelling implemented using LSTM (Long Short Term Memory) cells. 
1. __Conversion of Music to Textual Form__: Music is converted to ABC notation, which is a developed standard for sheet music. ABC notation interprets music in notes and bars form.
2. __Prediction of Characters using LSTM__: We predict character strings using LSTM cells which is converted back to error free and syntactically correct ABC notation.
3. __Conversion of ABC notation to Music__: The predicted ABC notation is then converted to Midi format using abc2midi package.

#### Music Samples
1. __Piano__: http://bit.ly/2qjwOKe
2. __Violin__: http://bit.ly/2qqX4h5
3. __Flute__: http://bit.ly/2roLLuM
4. __Guitar__: http://bit.ly/2r3J3Ik

#### Input and Dataset
A large array of midi files was collected and used as the input dataset. The midi files were converted to ABC files using abc2midi package. Then from each abc file, required midi program number’s (instrument) data was extracted and dataset for each instrument was created as a whole single file, called datafile.txt.

#### Cleaning the Dataset
This step involves removing unnecessary characters from the dataset so that training dataset size can be reduced resulting in faster epochs.
Following parts of dataset are removed :-
1. Statements which describe the ‘abc‘ file and not the music notes like X: (index),T: (title), comments.
2. Characters like ‘|’,’^’,’%’ etc.
3. Some expressions are resolved like a<b means a/2b3/2

#### Training
A specified length n of characters from the datafile is selected as a time sequence, and store the next character that occurred in the file as the output of that particular time sequence.
The new time sequence selected is the next n characters in the file, with the output of that time sequence being the next character after the time sequence.
This set of input time sequences (X_train), and their corresponding outputs (Y_train) are passed into the LSTM model implemented using a deep learning framework called Keras. This creates an LSTM model which is used to predict new characters for new time sequences. At the end, model file and trained weight files are extracted and saved.

#### Prediction
To predict music notations using LSTM cells built by Keras, we load the trained model file and weight file generated by training. We then provide a valid seed of a fixed sequence length which is randomly selected from our dataset file. This is done to maintain a logical consistency of seed with musical essence and coherency.
Then, for each time sequence, a new character is predicted by the LSTM Model. The 'n-1' characters of the last time sequence and the newly predicted character form the new time sequence for the purpose of prediction.

#### Convert predicted characters to correct ABC Notation
The predicted text is then converted to an error free ABC notation code to obtain an abc file. This file is fed to acb2midi package to convert to MIDI file. Following modifications are made to convert predicted text to correct abc file:-
1. Addition of bars (‘|’) after a certain number of notes which is decided by the meter of the song.
2. Modifying syntactically incorrect statements which involve numbers followed by numbers, note sharp(‘#’) and note flat(‘_’) characters not placed after valid notes etc. 
3. Adding the required file describing statements which were removed during cleaning.

#### GUI
Built using Tkinter

#### Screenshot

![](/Screenshot.jpg?raw=true)

#### References
1. http://karpathy.github.io/2015/05/21/rnn-effectiveness/
2. https://maraoz.com/2016/02/02/abc-rnn/
