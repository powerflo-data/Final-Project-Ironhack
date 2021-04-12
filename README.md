# Final-Project-Ironhack

For the final project @Ironhack (4 days work / 1 day presentation) I tried to predict the music-genre of a random song using the audio features spotify provides. <br>


![alt text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/research_question.JPG)

### Overview Spotify Audio Features:
![alt text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/audio_features.JPG)


Since this is a supervised machine learning approach (SL), it is necessary to have data about songs including their specific audio features and additional the genre of each song. <br>
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
               
Via the spotipy API, I used the names of each genre to find hundrets of playlist per genre, then gathered all the containing songs per playlists per genre. <br>
Result: more than 300k songs
  
# 1 Data Cleaning
There can be multiple genres assigned to an alblum or an artist. Since its the decision of the artist, there might be a couple of main-genres and subgenres assigned to the song. To continue working with only 18 main genres while there is only one genre assigned to each single song, I used a matching algorithm to decide which main-genre gets assigned to a song. I created a dictionary having 18 main-genres as keys and using sub-genres as values. The sub-genres I used for the dictionary are listed below each main-genre: [Wikipedia -> Musicgenre](https://de.wikipedia.org/wiki/Kategorie:Musikgenre) <br>


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


*(ranking the most likely main-genre by counting the number of full string matches within the genre feature of each song compared to the records inside the genre-dictionary mentioned above)

# 2 EDA:

During EDA, I checked the nature of the data using different plots from seaborn lib: distribution/aggregation/boxplots: <br>
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/danceability_genre_aggregation.png) <br>

Also I: <br>
- removed outliners
- checked multicorrelation via VIF, heat map, chi2
- dealt with data imbalance within the target feature (e.g. random sampling, smote)
- checked feature importance vs target value
- transformed some numerical values using sqrt
- encoded categorical features
- normalized the features using standardscaler
- tried feature engineering

![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/heatmap.png) <br>

Afterwards, the cleaned & processed data consisted of about 15k songs.

# 3 Modeling:

I tried a few different models with default parameters to check the underlying mathematical nature of the data. <br>
In between logistic regression, k-neighbors, decision tree and random forest, the latter got quite good results. <br>
For the random forest classifier, I tried to optimize some modeling parameters using gridsearch and cross validation methods from sklearn-lib. <br>
### Confusion Matrix (normalized: row-wise): <br>
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/Confusion_matrix_norm_random_forest.png) <br>

### Scores: <br>
***accuracy:  0.4612 <br>
Kappa:  0.4236 <br>
F1 score:  0.4422*** <br>

Although the accuracy is considered to be low when facing single class classification problems (less than 50%), the performance of the model isn't so bad. Taking the kappa score into account, the results are quite decent compared to random choice (a kappa score equal or less than 0.0 means a model-performances similar or worse than random choice). <br>


## Results: <br>

**Roughly 1 out of 2 songs will be predicted correctly (genre)**. <br>

**Feature importance:**
1) acousticness
2) danceability
3) energy
4) valence
5) speechiness




### Further investigation using UL: <br>
When looking at the confusion Matrix, it's noticeable that some genres will get predicted pretty good (e.g. Hip-Hop) while for others (e.g. Rock, Pop) the model performs way worse. <br> 

Taking a look into the target aggregated feature means, some genres are pretty well seperated from all others (e.g. Hip-Hop): <br>
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/energy_vs_speechiness_mean.png) <br>

Keeping this in mind, I build up an unsupervised learning model using the k-means clustering algorithm. Choosing 16 clusters as there are 16 main genres within the data. Interesting at this point: The silhoute score peaked using 16 clusters after continuing going down further.

### Clustering results: 

For some clusters, there are at least two or more different genres with a hugh amount of records represented, while for other clusters, there is only one genre very dominant among the others:
- **dominant genre in cluster 3:** *Funk*
- **dominant genre in cluster 6:** *Hip-Hop*
- **dominant genre in cluster 8:** *Punk* <br>

Distribution of the former target features (main-genre) among the calculated clusters:
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/kmeans_cluster_with_targets1.png)
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/kmeans_cluster_with_targets2.png)
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/kmeans_cluster_with_targets.png)
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/kmeans_cluster_with_targets4.png) <br>




Looking at the important features and plotting the calculated cluster centers shows interesting results:
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/energy_vs_speechiness.png) <br>
Hip-Hop for example (supposed to be cluster 6, green marker) is pretty much seperated from the other clusters but in this view, it is pretty near to cluster 10. Taking more features into account, makes it more easy to predict Hip-Hop compared to other genres. 


## Conclusion:

The short detour using **UL** brought up similar reasoning when it comes to separating particular genres from each other. <br>
Coming back to the **research question** of the project: *Is it possible to predict the genre of a song?* <br>
=> Based on the audio features from spotify: **Yes it is**. <br>
=> Though when wanting to recommend new music titles, it's probably better using clustering methods like in unsupervised learning approaches. <br>


### Further improvements:
- optimization of the algorithm used to assign a main-genre to a song
- handling of data imbalance within the target feature (classes)
- furter optimization of modeling parameter / model selection
