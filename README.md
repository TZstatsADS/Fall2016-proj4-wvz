# Project: Words 4 Music

### [Project Description](doc/Project4_desc.md)

![image](http://cdn.newsapi.com.au/image/v1/f7131c018870330120dbe4b73bb7695c?width=650)

Term: Fall 2016

+ [Data link](https://courseworks2.columbia.edu/courses/11849/files/folder/Project_Files?preview=763391) - (**courseworks login required**)
+ [Data description](doc/readme.html)
+ Contributor's name:  
    + Wanyi Zhang (wz2323)  
+ **Project title**: Association mining of music and text
+ **Project summary**: In this project, we will explore the association between music features and lyrics words from a subset of songs in the [million song data](http://labrosa.ee.columbia.edu/millionsong/). Based on the association patterns identified, we will create lyric words recommender algorithms for a piece of music (using its music features).
+ **Methodology**: The basic idea is to get 'doc_word' (expected word prob distribution for songs) by matrix multiplication of 'doc_topics' (topic probabilities for songs) and 'word_topics' (word prob for 20 topics). 'word_topics' matrix can be obtained by topic modeling on training data. 'doc_topics' matrix can be predicted from trained random forest model. [Detailed workflows](https://github.com/TZstatsADS/Fall2016-proj4-wvz/tree/master/lib) are as follows:
    + [Music features.Rmd] **Extracted 1125 music features from -/analysis/ part of 2350 .h5 files**:
        + 5 bar/beat/section/segment/tatum lengths (slopes in lm model)
        + 100 pitch_timbre features (using RGB feature extraction method),
        + 1000 loudness (using RGB feature extraction method), 
        + 20 confidence (median, SD, IQR, length of 5 confidence variables)
    + [Lyrics process.Rmd] **Cleaned text data of lyrics for 2350 songs, converted word counts to binary existence**
        + Ranked words by total frequency of all 2350 songs as baseline ranking
    + [Topic Modeling_Binary.Rmd] **Conducted topic modeling on binary text data**
        + Obtained 20 topics, visualized results interactively
        + Obtained 2 matrices:
            + 'doc_topics' (topic probabilities for 2350 songs), 
            + 'word_topics' (word prob for 20 topics),
        + Obtained 'doc_word' (expected word prob distribution for 2350 songs) by matrix multiplication of 'doc_topics' and 'word_topics'
        + Tested if 'doc_word' is a good prediction for word distribution in songs by comparing to baseline ranking (better for 97.6% songs)  
*By training topic model we got 'word_topics' matrix (matrix of word probabilities for 20 topics). As for 'doc_topics' matrix for new songs, we will predict it by finding links between music features and 'doc_topics' matrix! Therefore next we are going to train random forest model for such links.*
    + [Random forest.Rmd] **Trained random forest model**
        + Set aside 10% training data as validation set
        + Trained random forest with tuning parameter 'mtry'
        + Predicted 'doc_topics' matrix for validation set by using trained model
        + Obtained 'doc_word' matrix by multiplying predicted 'doc_topics' and 'word_topics'
        + Ranked words using 'doc_word' matrix for validation set
        + Tried average of rankings of baseline and our model
        + Tested ranking prediction for 235 validation songs
    + [Testing.Rmd] **Final testing on 100 test songs**
        + Extracted 1125 music features 
        + Predicted 'doc_word' matrix for word probability distribution of 100 test songs
        + Filled empty words in 'doc_word' and reorder the columns as in sample_submission.csv
        + Predicted word ranking for 100 test songs, and wrote in .csv format
        
        

Following [suggestions](http://nicercode.github.io/blog/2013-04-05-projects/) by [RICH FITZJOHN](http://nicercode.github.io/about/#Team) (@richfitz). This folder is orgarnized as follows.

```
proj/
├── lib/
├── data/
├── doc/
├── figs/
└── output/
```

Please see each subfolder for a README file.
