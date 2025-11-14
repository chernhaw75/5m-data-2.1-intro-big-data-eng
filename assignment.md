# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: From the `movies` collection, return the documents with the `plot` that starts with `"war"` in acending order of released date, print only title, plot and released fields. Limit the result to 5.

Answer:

```python

query = {"plot": {"$regex": r"^War ", "$options": "i"}}  # case-insensitive

# Display the results
for m in movies.find(query).sort("year", -1).limit(5):
    print (m)

```

### Question 2

Question: Group by `rated` and count the number of movies in each.

Answer:

```python

pipeline = [
    {"$group": {"_id": "$rated", "count": {"$sum": 1}}},
    {"$sort": {"count": -1}}  # optional: sort by count descending
]

results = movies.aggregate(pipeline)

for r in results:
    print(f"Rated: {r['_id']}, Count: {r['count']}")


```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python


pipeline = [
    {
        "$lookup": {
            "from": "comments",
            "localField": "_id",
            "foreignField": "movie_id",
            "as": "movie_comments"
        }
    },
    {
        "$match": {
            "$expr": {"$gt": [{"$size": "$movie_comments"}, 3]}
        }
    },
    {
        "$project": {
            "_id": 0,
            "title": 1,
            "num_comments": {"$size": "$movie_comments"}
        }
    }
   
]

results = db.movies.aggregate(pipeline)

for movie in results:
    print(movie)


```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
