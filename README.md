# correct_twitter2019-Project-and-Analysis
Step-by-Step Guide to Set Up the System:
1. Install Python and Dependencies:
First, you need to make sure that you have Python and the necessary libraries installed on your machine.

Install Python:
Make sure to check the box to add Python to your system PATH during installation.

Install Required Libraries:
Open a terminal or command prompt and install the required Python libraries by running:
pip install pandas matplotlib

2. Download the Dataset:
You need to have a dataset file named correct_twitter_201904.tsv. This file should contain Twitter data with columns like text, created_at, author_id, like_count, place_id, etc.

3. Python Code Implementation:
You will need to write the Python code that I provided in one of your files. You can create a new Python file (twitter_analysis.py) and copy-paste the code snippets below.

Full Code Implementation:

import pandas as pd
import matplotlib.pyplot as plt

# Load the dataset
df = pd.read_csv('correct_twitter_201904.tsv', sep='\t')
df['created_at'] = pd.to_datetime(df['created_at'], errors='coerce', utc=True)
def tweets_per_day(term):
    filtered_df = df[df['text'].str.contains(term, case=False, na=False)]
    daily_tweet_count = filtered_df.groupby(filtered_df['created_at'].dt.date)['id'].count()
    return daily_tweet_count.reset_index(name='tweet_count')
def unique_users_posting_term(term):
    filtered_df = df[df['text'].str.contains(term, case=False, na=False)]
    unique_user_count = filtered_df['author_id'].nunique()
    return unique_user_count
def average_likes_for_term(term):
    filtered_df = df[df['text'].str.contains(term, case=False, na=False)]
    average_likes = filtered_df['like_count'].mean()
    return average_likes
def place_ids_for_term(term):
    filtered_df = df[df['text'].str.contains(term, case=False, na=False)]
    unique_place_ids = filtered_df['place_id'].dropna().unique()
    return unique_place_ids
def times_of_day_for_term(term):
    filtered_df = df[df['text'].str.contains(term, case=False, na=False)]
    tweet_hours = filtered_df['created_at'].dt.hour
    hour_distribution = tweet_hours.value_counts().sort_index()
    return hour_distribution
def most_frequent_user_for_term(term):
    filtered_df = df[df['text'].str.contains(term, case=False, na=False)]
    tweet_counts_by_user = filtered_df['author_id'].value_counts()
    top_user = tweet_counts_by_user.idxmax()
    top_user_tweet_count = tweet_counts_by_user.max()
    return top_user, top_user_tweet_count
term = "music"

print("Tweets per day containing the term:")
print(tweets_per_day(term))

print("\nNumber of unique users posting tweets containing the term:")
print(unique_users_posting_term(term))

print("\nAverage number of likes for tweets containing the term:")
print(f"{average_likes_for_term(term):.2f}")

print("\nPlace IDs where tweets containing the term came from:")
print(place_ids_for_term(term))

print("\nNumber of tweets posted at each hour of the day:")
print(times_of_day_for_term(term))

print("\nUser who posted the most tweets containing the term:")
user, count = most_frequent_user_for_term(term)
print(f"User ID: {user} with {count} tweets.")

4. Run the Python Script:
Open your terminal or command prompt and navigate to the folder where the Python file (twitter_analysis.py) and dataset are located.

Run the Python file using:
python twitter_analysis.py

Design Justifications:

Pandas Library:
Pandas is ideal for loading, manipulating, and querying structured data like CSV or TSV files. It has powerful functions like .str.contains(), .groupby(), and .value_counts() that make querying and data analysis efficient.
Alternative: SQL databases could be used for larger datasets, but for ease of use and quick analysis, Pandas provides more flexibility for smaller datasets.

Datetime Handling:
It's important to convert the created_at column to a datetime format for time-based analysis, like grouping by day or extracting the hour of the tweet. Without this conversion, extracting date and time information would be error-prone.

Value Counts and Groupby:
This function quickly summarizes the distribution of a column's values, such as the number of tweets per hour or tweets per user. It's highly efficient for frequency analysis.
Grouping allows us to aggregate data over a specific period, such as tweets posted per day. This helps in time-based trend analysis.

Matplotlib for Visualization:
If you want to visualize trends (e.g., tweet distribution over the day), Matplotlib is an excellent choice for basic plots like bar charts. It integrates seamlessly with Pandas.

Modular Functions:
Each query is encapsulated in its own function to make the code reusable and clean. You can call each function independently to answer specific questions.
Expected Output:
