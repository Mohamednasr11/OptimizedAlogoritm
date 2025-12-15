# Students names and ids

- Student 1: [Mohamed nasr eldeen], [1000287495]
- Student 2: [Mohamed Ali Rizk], [1000288299]
- Student 3: [Name], [ID]
- Student 4: [Name], [ID]
- Student 5: [Name], [ID]

Sentence Relevance Sorting Algorithm

This project provides an optimized solution for sorting a list of sentences based on their relevance, determined by the count of specific "good words" they contain.

---

## Problem Description Naive

This algorithm sorts or finds the best review based on the number of goodWords.
It uses nested loops to compare each word with all words in the goodWords list.
This makes it simple but inefficient for large datasets.

## The Naive Solution

The naive algorithm finds the best sentence by comparing every word with every word in the `goodWordsList` using `nested loops`.

1.  **Nested Word Comparison**: Each word in a sentence is checked against all words in `goodWordsList`.

2.  **On-the-fly Scoring**: Scores are calculated during the comparisons, instead of pre-computing them.

3.  **Best Review Selection**: The review with the highest score is chosen; ties go to the first encountered.

> **Simple but inefficient for large datasets.**

## Complexity Analysis Naive

This naive approach is simple but less efficient due to nested loops:

- **Time Complexity**: `O(n * m * g)`

  - `n`: Number of Reviews
  - `m`: Average number of words per Review
  - `g`: Number of words in `goodWordsList`
  - Each word in every Review is compared with all words in the goodWordsList.

- **Space Complexity**: `O(1)`
  - Only counters are used (`score`, `max_score`, `best`), no extra storage.

---

## Pseudocode Naive

```
score = 0
max_score = 0
best = 0

for n = 0 to reviews.length - 1
    score = 0

    for m = 0 to reviews[n].length - 1
        for g = 0 to goodWordsList.length - 1
            if reviews[n][m] == goodWordsList[g]
                score = score + 1
            end if
        end for
    end for

    if score > max_score
        max_score = score
        best = n
    end if
end for

return best
return reviews[best]

```

## Problem Description Optimized

The primary goal is to rank sentences by the number of important words they feature.
Sentences with a higher score (more good words) should appear first.
A crucial secondary requirement is to maintain the original relative order of sentences that have the same score (a stable sort).

## The Optimized Solution

To tackle this efficiently, the algorithm avoids the common pitfalls of naive solutions (slow lookups and repetitive calculations) by implementing a three-part strategy:

1.  **Instant Word Lookup**: The list of `goodWords` is converted into a `HashSet`. This allows for checking if a word is "good" in constant time, O(1), which is a massive improvement over searching through a list.

2.  **Pre-computation of Scores**: Instead of recalculating scores during the sorting comparison, the algorithm iterates through each sentence **once** to compute its relevance score. This score, along with the sentence's original position in the list, is stored.

3.  **Stable Sorting Logic**: The list is sorted based on the pre-computed scores in descending order. If two scores are equal, their original indices are used as a tie-breaker, thus preserving their initial order.

---

## Complexity Analysis Optimized

This optimized approach leads to a significant performance gain:

- **Time Complexity**: `O(n * m + n log n)`

  - `O(n * m)`: To iterate through `n` sentences, each having an average of `m` words, for score computation.
  - `O(n log n)`: For a single, efficient sort operation on the computed scores.

- **Space Complexity**: `O(n + g)`
  - `O(n)`: To store the score and original index for each sentence.
  - `O(g)`: To store the `g` good words in the `HashSet`.

---

## Pseudocode Optimized

```
Algorithm SortSentencesByGoodWords(sentences, goodWords)
1. Convert goodWords into a set goodWordsSet
2. Create an empty list rankList
3. For each sentence i in sentences:
     a. score ← 0
     b. Split sentence i into words
     c. For each word:
          If word ∈ goodWordsSet:
              score ← score + 1
     d. Add (score, i) to rankList
4. Sort rankList by descending score,
   and by ascending original index if scores are equal
5. Return sentences ordered according to rankList
End Algorithm
```

---

## How to Run

The algorithm is implemented in Dart. You can run the `main` function in `bin/algoritm2.dart` to see an example in action.

```dart
// Example Usage:

void main() {
  final algorithm = OptimizedAlgorithm();

  List<String> sentences = [
   'This is a great movie',
    'Exciting and thrilling experience',
    'A wonderful journey of love and friendship',
    'An average film with some good moments',
    'A fantastic adventure full of surprises'
  ];

  List<String> goodWords = [
    'great',
    'exciting',
    'thrilling',
    'wonderful',
    'love',
    'fantastic',
    'adventure'
  ];

  final result = algorithm.sortSentences(sentences, goodWords);

  // Expected Output:
  // [ 'A fantastic adventure full of surprises',score:2
  //   'Exciting and thrilling experience',score:2
  //   'A wonderful journey of love and friendship',score:2
  //   'This is a great movie', score:1
  //   'An average film with some good moments' score:0
  //   ]
  print(result);
}
```
