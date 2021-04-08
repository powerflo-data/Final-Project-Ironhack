# Final-Project-Ironhack

For the final project @Ironhack I tried to predict the music-genre of a random song using the audio features spotify provides. <br>


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
(ranking the most likely main-genre by counting full string matches comparing the genre of each song with the dictionary mentioned above)
![alt_text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/main_genre_assignment.JPG) <br>
Note: This approach is heavily impacting the nature of the data and is adding a bias to it.  <br>

Due to the logic of the assigning alrotihm, there was no song left having "Rock ‚n‘ Roll" or "Disco" assigned to it. They either got "Rock" or "Pop"/"Hip-Hop" as genre assigned to. <br>
Therefore I continued with only 16 main-genres. <br>

Afterwards I dealt with typical cleaning processes like:
- handling of NaN values
- removing duplicates
- checking data-types
- checking target feature distribution
- dropping irrelevant features (e.g. artistname, songname)

# 2 EDA:


-removed outliners
-dealt with data imbalance within the target feature
-checked multicorrelation
-



i was left with around 

