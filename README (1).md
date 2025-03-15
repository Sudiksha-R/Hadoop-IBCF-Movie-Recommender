
# Movie recommender system Readme

This program creates Movie recommender system using Pyspark. the following mapper and reduce functions have been used:- 
1. This focuses on reading the raw rating data and aggregating it by userId.

Mapper:

Reads the raw rating data in the format "userId,movieId,rating"
Output: <userId, movieId:rating>
Reducer:

Aggregates the rating data by userId

Output: <userId, movieId1:rating1, movieID2:rating2> 
Co-occurrenceMatrixGenerator

This workflow generates the co-occurrence matrix based on the aggregated rating data.

Mapper:

Reads the aggregated rating data
Input: <offset, userId \t movieId1:rating1,movieId2:rating2,...>
Output: <movieIdi:movieIdj, 1>
Reducer:

Generates the co-occurrence of each movieId pair
Input: <movieIdi:movieIdj, (1, 1, ...)>
Output: <movieIdi:movieIdj, 1+1+...>
Workflow 3: Co-occurrenceMatrixNormalization

This workflow normalizes the co-occurrence matrix.

Mapper:

Reads the unnormalized co-occurrence matrix
Input: <offset, movieIdi:movieIdj \t count>
Output: <movieIdi, movieIdj:count>
Reducer:

Normalizes the row of movieIdi
Input: <movieIdi, (movieIdj1:count1, movieIdj2:count2, ...)>
Output: <movieIdj, movieIdi=countj/totalCount>
Workflow 4: UserRatingAveraging

This workflow calculates the average rating given by each user.

Mapper:

Reads the raw rating data in the format "userId,movieId,rating"
Input: <offset, userId,movieId,rating>
Output: <userId, rating>
Reducer:

Calculates the average rating given by a user
Input: <userId, (rating1, rating2, ...)>
Output: <userId, (rating1 + rating2 + ...) / #rating>
Workflow 5: MatrixCellMultiplication

This workflow performs matrix cell multiplication by combining the normalized co-occurrence matrix and the rating matrix.

Mapper1:

Reads the normalized co-occurrence matrix
Input: <offset, movieIdk \t movieIdi=weighti>
Output: <movieIdk, movieIdi=weighti>
Mapper2:

Generates the rating matrix by reading the raw rating data
Input: <offset, userId,movieId,rating>
Output: <movieId, userId:rating>
Reducer:

Multiplies a cell of the co-occurrence matrix with the corresponding cell of the rating matrix
Reads the average rating for each user: `HashMap<userId,>

Metrics and Visualizations:
In addition to the MapReduce workflows, I have computed metrics (such as RMSE) and to generate visualizations that provide insights into the data distribution and relationships.







