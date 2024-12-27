# Detailed Report: Book Recommendation System Using Collaborative Filtering

This project implements a book recommendation system using collaborative filtering based on user ratings. The system processes datasets of books, users, and book ratings to create a model that can suggest books similar to those a user has already rated. The recommendation is achieved by leveraging nearest neighbors and handling various data preprocessing steps, including filtering and feature engineering.

## Step 1: Setup and Library Imports

In the first step, several Python libraries were imported, including:

    pandas: for data manipulation.
    numpy: for numerical operations.
    matplotlib and seaborn: for data visualization.
    IPython: for displaying dataframes in a Jupyter notebook.
    scipy.sparse and sklearn.neighbors: for handling sparse matrices and applying the nearest neighbors algorithm.
    pickle: for saving the trained model for future use.

## Step 2: Data Loading + Cleaning

Book Dataset: The BX-Books.csv file, containing information about books (ISBN, title, author, publisher, year of publication, etc.), was loaded and cleaned as follows:

    Invalid Year-Of-Publication values were replaced with NaN.
    Years outside the range 1900-2024 were also replaced with NaN.
    Missing values were filled with "Unknown."
    Non-numeric values were coerced to NaN in the Year-Of-Publication column.
    The dataset was reduced to relevant columns, renamed to user-friendly names, and the Year-Of-Publication column was converted to strings for easier processing.

User Dataset: The BX-Users.csv file, containing user information (user ID, location, and age), was loaded and cleaned as follows:

    Invalid or missing values in the Age column were handled by filling with "Unknown."
    Age values were restricted to the realistic range of 2 to 120 and converted to integers where applicable.

Rating Dataset: The BX-Book-Ratings.csv file, containing ratings by users for various books, was loaded and cleaned as follows:

    Invalid ratings outside the range of 0-10 were converted to NaN.
    Missing or invalid ratings were filtered out, and remaining ratings were converted to integers.

After cleaning, the relevant columns were renamed consistently across all datasets:

    Books: isbn, title, author, year, publisher, image_url
    Users: user_id, location, age
    Ratings: user_id, isbn, book_rating

## Step 3: Data Filtering and Feature Engineering

Filtering Users and Books:

    Users who had rated more than 200 books were retained to focus on active users and reduce noise.
    Books with fewer than 50 ratings were removed to ensure better quality recommendations.

Merging Data:

    The ratings data was merged with the books data to associate each rating with its corresponding book details.
    A new column was added to track the number of ratings for each book.
    Only books with more than 50 ratings were kept for further analysis.

Removing Duplicate Ratings:
    
    Duplicate ratings (where the same user rated the same book multiple times) were removed to avoid biasing the recommendation system.

## Step 4: Pivoting Data for Collaborative Filtering

To make book recommendations based on user ratings, we first created a pivot table. This allows us to organize the data in a way that can be easily used for finding similarities between books.

A pivot table was created where:

    Rows represent book titles.
    Columns represent user IDs.
    Values represent book ratings.

Missing ratings were filled with 0 to indicate that a user has not rated a particular book.

## Step 5: Training the Nearest Neighbors Model

Next, we converted the pivot table into a sparse matrix using csr_matrix. This transformation helps efficiently store and process the data, as it reduces memory usage by only storing non-zero ratings.

Finally, we trained a Nearest Neighbors model using the sparse matrix. The model utilizes the Euclidean distance metric to find similar books based on the ratings provided by users.

## Step 6: Book Recommendation Testing

Two tests were performed to evaluate the recommendation system:

    A book titled "Harry Potter and the Chamber of Secrets" was used to check for its nearest neighbors.
    A function recommend_book was created to allow the system to recommend books based on a given book name. For example, the book "To Kill a Mockingbird" was used to generate recommendations.

## Step 7: Saving the Model

The trained model, along with essential data (book names, book pivot data, and final ratings), will be saved as pickle files. This enables the model to be easily loaded and used for making future book recommendations in a web app.
