# Final-Project-Ironhack

For the final project @Ironhack (4 days work / 1 day presentation) I tried to predict the music-genre of a random song using the audio features spotify provides. <br>


![alt text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/research_question.JPG)

### Overview Spotify Audio Features:
![alt text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/audio_features.JPG)


Since this is an supervised machine learning approach (SL), it is necessary to have data about songs coming with their specific audio features and additional the genre of each song. <br>
<br>
Therefore I used the spotipy API to: <br>
- scrape thousands of songs and their audio features <br>
- retrieve the genre of each song via the artist or the album (the genre on spotify is only connected via the artist or the album of a song) <br>
   
    
# 0 Data Gathering 
I focused on 18 different (main) genres, since this has been a good aggregation of all music-subgenres out there.   [Wikipedia -> Musicgenre](https://de.wikipedia.org/wiki/Kategorie:Musikgenre)

```
music_genre = ["Blues","Country", "Dark Music","Disco", "Electro","Folk", "Funk", "Hip-Hop","Jazz","Latin", "Metal", 
               "Pop","Punk","Reggae","Rock", "Rock ’n’ Roll", "Ska", "Traditional"]
```
               
Via the spotipy API, I used the genre names to find multiple playlist for each genre, then gathered all the containing songs per playlists per genre. <br>
Result: more than 300k songs
  
# 1 Data Cleaning
There can be multiple genres assigned to an alblum or an artist. Since its the decision of the artist, there might be even main and subgenres assigned to the song. To continue working with only 18 main genres while there is only one genre assigned to each single song, I used a matching algorithm to decide which main-genre gets assigned to a song. I created a dictionary having 18 main-genres as keys and using sub-genres as values. The sub-genres I used are listed below each genres: [Wikipedia -> Musicgenre](https://de.wikipedia.org/wiki/Kategorie:Musikgenre) <br>


### Assigning the main-genre:
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/main_genre_assignment.JPG) <br>
Note: This approach is heavily impacting the nature of the data and is adding a bias to it.  <br>

Due to the logic of the assigning alrotihm*, there was no song left having "Rock ‚n‘ Roll" or "Disco" assigned to it. They either got "Rock" or "Pop"/"Hip-Hop" as genre assigned to. <br>
Therefore I continued with only 16 main-genres. <br>

Afterwards I dealt with typical cleaning processes like:
- handling of NaN values
- removing duplicates
- checking data-types
- checking target feature distribution
- dropping irrelevant features (e.g. artistname, songname)


*(ranking the most likely main-genre by counting full string matches comparing the genre of each song with the dictionary mentioned above)

# 2 EDA:

During EDA, I checked the nature of the data using different plots from seaborn lib: distribution/aggregation/boxplots: <br>
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/danceability_genre_aggregation.png) <br>

Also I: <br>
- removed outliners
- checked multicorrelation via VIF, heat map, chi2
- dealt with data imbalance within the target feature (e.g. random sampling, smote)
- transformed some numerical values using sqrt
- encoded categorical features
- normalized the features using standardscaler

![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/heatmap.png) <br>

Afterwards, the cleaned & processed data consisted of about 15k songs.

# 3 Modeling:

I tried a few different models with standard parameters to check the underlying mathematical nature of the data. <br>
In between logistic regression, k-neighbors, decision tree and random forest, the latter got quite good results. <br>
For the random forest classifier, I tried to optimize some modeling parameter using gridsearch and cross validation methods from sklearn-lib. <br>
### Results: <br>

![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/Confusion_matrix_random_forest.png) <br>

accuracy:  0.4612
Kappa:  0.4236
F1 score:  0.4422
