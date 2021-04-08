# Final-Project-Ironhack

For the final project @Ironhack I tried to predict the music-genre of a random song using the audio features spotify provides. <br>


![alt text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/research_question.JPG)

# Overview Spotify Audio Features:
![alt text](https://github.com/powerflo-data/Final-Project-Ironhack/blob/main/audio_features.JPG)


Since this is an supervised machine learning approach (SL), it is necessary to have data with songs and their audio features and additional the genre of each song. <br>
<br>
Therefore I used the spotipy API to: <br>
- scrape thousands of songs and their audio features <br>
- retrieve the genre of each song via the artist or the album (the genre on spotify is only connected via the artist or the album of a song) <br>
   
    
# 0 Data Gathering
I focused on 18 different genres, since this has been a good aggregation of all music-subgenres out there.

'''music_genre = ["Blues","Country", "Dark Music","Disco", "Electro","Folk", "Funk", "Hip-Hop","Jazz",
               "Latin", "Metal", "Pop","Punk","Reggae","Rock", "Rock ’n’ Roll", "Ska", "Traditional"]'''
Via the spotipy API, I used the genre names to find multiple playlist for each genre, then gathered all the containing songs per playlist.
  
