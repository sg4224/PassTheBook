# PassTheBook

Final project for the Building AI course

## Summary

PassTheBook is a community app where people list books they've finished and pass them on to friends or neighbours. AI matches each book with the nearby reader most likely to enjoy it, helping people save money and keep books in use.


## Background

Many people buy a book, read it once, and leave it on a shelf for years. Meanwhile, others can't afford new books or can't find them nearby. Passing books on already happens informally, but it's hard to know who wants which book, who is close by, and who currently has it.

Problems this idea addresses:
* Books are expensive, especially for students and people on small budgets
* Finished books sit unused on shelves
* It's hard to find the right person to pass a book to
* There is no easy way to track where a shared book has gone

Sharing books saves money, reduces waste, and helps build a reading culture in a community.


## How is it used?

1. A person finishes a book and scans its barcode (or types the title) to list it as "available to pass on".
2. The app suggests friends or nearby readers who would enjoy it, based on their interests and wishlists.
3. The chosen reader accepts and collects the book.
4. After reading, they pass it on again, and the book's journey is recorded.

**Users:** students, friends, book clubs, neighbours, and anyone who wants to read more without spending a lot. The app should work well on a basic smartphone, support several languages, and let people choose safe, public places for handoffs.

Other people affected include libraries, authors, and bookstores. Book sharing can also spread word of mouth and grow readership, which benefits them.


## Data sources and AI methods

**Data sources**
* Book details (title, author, genre, description) from open sources such as [Open Library](https://openlibrary.org/developers/api)
* Users' reading history, ratings, and wishlists (collected in the app, with consent)
* Community or area (e.g. same school or neighbourhood)
* Feedback after each handoff (book condition, did they enjoy it)

**AI methods**
* **Content-based recommendation:** compares a book's description with a reader's interests
* **Collaborative filtering:** recommends books liked by similar readers
* **Natural language processing (TF-IDF):** turns text descriptions into numbers so they can be compared
* **Computer vision / OCR:** reads barcodes and covers to add books quickly

**Small demo:** this example ranks readers for a book using cosine similarity between the book description and each reader's interests.

```
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

book = "A detective investigates a mysterious murder in a small coastal town"

readers = pd.DataFrame({
    "name": ["Asha", "Ben", "Chen"],
    "interests": [
        "crime mystery detective thriller",
        "space science fiction aliens",
        "murder mystery small town stories",
    ],
})

vectorizer = TfidfVectorizer()
vectors = vectorizer.fit_transform([book] + readers["interests"].tolist())

scores = cosine_similarity(vectors[0], vectors[1:])[0]
readers["match_score"] = scores

print(readers.sort_values("match_score", ascending=False))
```

| Syntax | Description |
| ------ | ----------- |
| Input | Book description, reader interests |
| Method | TF-IDF + cosine similarity |
| Output | Ranked list of best-matching readers |


## Challenges

* It relies on trust: the app can't make sure books are returned or passed on.
* New users have little data, so early recommendations will be weak (the "cold start" problem).
* Barcode and cover scanning can make mistakes, especially with older or non-English books.
* It only works well where enough people in an area take part.
* People without smartphones or without users nearby can't benefit.
* Location and reading habits are personal data, so privacy, consent, and safe meetups are essential.
* It doesn't address the effect on authors and publishers, or copyright questions.


## What next?

* Build a simple working prototype and test it with friends or a class
* Add reading groups, reviews, and discussion for each book
* Partner with schools, libraries, and community centres
* Add badges or rewards for people who share often
* Support more languages and regional books
* Organise local book swap events and a donation option

To do this I would need to learn app development, get feedback from real users, and get advice on data privacy.


## Acknowledgments

* Elements of AI and Building AI courses (University of Helsinki and MinnaLearn / Reaktor)
* [Open Library](https://openlibrary.org/) for open book data
* Python libraries: pandas and scikit-learn
* Inspiration: BookCrossing, Little Free Library, and Goodreads
