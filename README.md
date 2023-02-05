# Data Analysis: movie characterisics and streaming platform

This data analysis is examining if there is a connection between various streaming services and specific traits of the movies they offer. The factors being considered include movie ratings on IMDb, genre, length, year of release, country of release, and others, as it is believed these may vary between platforms and have an impact on the predictions. The aim of this data analysis is to predict whether a movie title is available on Netflix or not.

**Sources:**

1) Kaggle movie dataset 
    - The dataset contains a collection of movies available on the various streaming platforms
    - The data has been scraped from Netflix, Prime Video, Hulu and Disney+
    - https://www.kaggle.com/ruchi798/movies-on-netflix-prime-video-hulu-and-disney
    
2) IMDb rating dataset
    - The dataset contains movie ratings ranging from 1 to 10


## Columns
- ID: Movie ID
- Title: Movie Title
- Year: Release Year
- Age: Age restriction
- IMDB: IMDB Rating
- Rotten Tomatoes: Tomatoes Rating
- Netflix, Hulu,Prime Video, Disney+: Streaming Service Platforms
- Type: Movie Genres (empty)
- Genres: Movie Genres
- Directors: Movie Director
- Country: Release in Country
- Language: Available languages/translations
- Runtime: duration of each title in minutes



## Preprocessing

Delete columns or rows, which do not contain enough useful data f.e. nan values and cannot be easily replaced data without further investigation.
- The 'Type' column gets drop, because it provides no variance (constant value).
- The 'Rotten Tomatoes' column gets dropped, as it contains two thirds nan values and a very similar rating is provided through the IMBd column
- Rows that contain nan values for the columns 'IMDb', 'Directors', 'Genres', 'Country' and 'Language' get dropped as well, as
    * there is no easy way to substitute this data without additional data (all columns)
    * substitution with mean, median or 0 values could result into falsifying the data (IMDb)
    * one-hot-encoding would result in a to big dataframe which would tremendously increase the processing time for predictions (directors, language)

One hot encoding gets applied on the columns "Age", "Genre" and "Country" (country gets mapped to continents first before performing one hot encoding)

As visualized in the pandas profile report, there is a negative correlation between the columns Netflix and Prime Video, which leads to the conclusion that their provided services differ a lot.
For an even better comparison, titles that are both on netflix and prime (323) get removed in the preprocessing. There are 2915 unique titles on Netflix and 11023 titles on Prime Video.
The least amount of titles are available on the smaller platforms Hulu and Disney+.

We combine the unique titles on Netflix with the unique titles on Prime Video and create a "onNetflix", which will be used for classification / prediction later on.



## Conclusion

The models for logistic regression and decision tree classification appear to perform well when only taking accuracy scores into account. However when we compare the values to the R2 scores we see that the predictions of the models are not reliable.

Similarly a Neural Networks do not seem to be suitable for prediction allthough R2 (35 %) for train and test dataset is better in comparison to the models using logistic regression and the decision tree classifier. KNN has a similar R2 for the train dataset however performs quite badly for the R2 of the test dataset (-80%).

When comparing the plots of PCA and t-SNE we can see that t-SNE can visualize data in a more clustered way that makes interpretation easier. After dimensionality reduction using PCA there are still 35 components (51 before PCA) needed for an explained variance of 95%. 

It was interesting to see the effect of combinin PCA with TSNE on the plots as interesting shapes where formed.

k-Means and spectral clustering show that there exist many clusters. A reason for the high amount of clusters might be - as previously stated - the relatively high number of components that the cluster algorithms had to deal with, even after dimensionality reduction was applied to the dataset. As the data points are pretty close to each other interpretation is quite tricky for these plots.
Intrestingly while k-Means found increasingly more clusters the higher the value for k, higher values for k showed less inpact on the amount of clusters for spectral clustering which stuck to two clusters for all the used k.

_Allthough there is a negative correlation between titles available on Netflix and Prime Video, the overall results suggest that the data available is **not suitable** for the predictions regardless of the different approaches used._

The results could most likely be improved using additional data or applying different preprocessing steps.