# Table of Contents
* [Motivation](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#motivation)

* [Data Collection](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#data-collection)

* [Data Description + Intial thoughts](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#Data-Description-+-Initial-thoughts)

## Motivation
Just in the past year I had released 2 songs on spotify. As far as marketing went I felt it just bounced around my friends and back. As I forayed into Data Science, I realized that Spotify analyzes each track & assigns it a score.  So what my main motive was to see if my song 'lacked' in some elements according to spotify. To do this I wanted to create a model which would predict the popularity score assigned by spotify for an unknown track.

## Data Collection
I collected the data using the spotipy library built around the spotify API

I intially queried the song's by year ( 2000 was the limit per year ) & maximum items returned per call was 50, but you could offset the index of the first result which;;l. To make the data base large enough I collected samples from 1980 - 2020 ( ~80000 data points).The other function I defined was to collect audio features by spotify for each track based on the track id.

* https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-features/ ( end point for audio features )
* https://spotipy.readthedocs.io/en/2.14.0/ ( spotipy library )

## Data Description + Intial thoughts

### Description of the Data Collected ( according to spotify docs) :
|Attribute |Description|
|--- | --- |
|1 - Acousticness (float) | A confidence measure from 0.0 to 1.0 of whether the track is acoustic.|
|2 - Danceability (float)| Danceability describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength, and overall regularity.|
|3 - duration_ms (int)| Duration of the track in ms|
|4 - energy (float)| Energy is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy.|
|5 - instrumentalness (float) | Predicts whether a track contains no vocals. “Ooh” and “aah” sounds are treated as instrumental in this context. 
|6 - key (int)| The estimated overall key of the track.|
|7 - liveness (float)||Detects the presence of an audience in the recording. Higher liveness values represent an increased probability that the track was performed live.| 
|8 - loudness| The overall loudness of a track in decibels (dB)
|9 - mode (int)| Mode indicates the modality (major or minor) of a track, the type of scale from which its melodic content is derived.|
|10 - speechiness (float)|  Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value.
|11 - tempo (int)| The overall estimated tempo of a track in beats per minute (BPM). In musical terminology, tempo is the speed or pace of a given piece and derives directly from the average beat duration.
|12 - time signature (int)|  An estimated overall time signature of a track.| 
|13 - valence (float) |  A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).| 

Apart from this I added another data point 'Numer of Markets' the song was released in. Country wise list was available from the 'search' end point
As a musician some of the data points already made sense to me. For instance the 'key' , 'mode' ,'time signature' , ' tempo' are familiar terms. Now some of the abstract and insteresting terms are 'valence' , 'instrumentalness' , 'energy' ,'Danceablity' are kind of vague. It will be interesting to see if these features are contributing to overall popularity. Since we have no way of physically changing the 'valence' as we do for the 'key' of the song.








