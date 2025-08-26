# Precision vs Recall

## Precision
Out of all the results the system returned, **how many were actually relevant?**

Precision = Relevant document retrived/Total documents retrived

### Intuition
* High precision -> fewer false positives (we don't see junk in results)
* Ex: If i search for "Machine learing" and system returns 10 results.
  * 8 are truly about ML, 2 are unrelated.
  * Precision = 8/10 = 0.8 (80%)

## Recall
Out of all the relevant documents in the system, **how many did the search return?**

Recall = Relevant document retrived/Total relevant documents in system

### Intuition
* High recall -> fewer false positives (we don't miss useful results)
* Ex: There are 50 documents in the system about ML
  * Our search retrived 8 of them
  * Recall = 8/50 = 0.8 (0.16%)

## Precision vs Recall Trade off

* High precision, low recall: We get only very accurate results, but might miss many relevant ones (Like a strict librarian giving us only the most relevant 2 books ignoring others)
* High recall, low precision: We get most of the relevant results, but also lots of irrelevant ones (Like google returning 10,000 results, many vaugely related)
* Search engines (OpenSearch, Google etc) try to balance both.
* In some domains, one is prioritized over the other:
  * Medical Search -> high recall (don't miss any relevant info)
  * Legal discovery / e-commerce product search -> high precision (don't show junk)
  