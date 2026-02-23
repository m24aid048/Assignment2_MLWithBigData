# Assignment2_CSL7110
By: **Shashi Saurav** (M24AID048)
GitHub Link: - https://github.com/m24aid048/Assignment2_MLWithBigData.git

**What were the Jaccard similarities, MinHash approximations, and LSH performance on the document datasets?**
    *   **Jaccard Similarities (char 3-grams)**: D1-D2 (0.978), D1-D3 (0.580), D1-D4 (0.305), D2-D3 (0.568), D2-D4 (0.306), D3-D4 (0.312).
    *   **MinHash Approximations (D1-D2, char 3-grams)**: Varied from 0.95 (t=20) to 1.0 (t=60). The approximate Jaccard similarity for D1-D2 with t=600 was 0.9783, very close to the exact Jaccard of 0.978. Experiments for best `t` found average similarities ranging from 0.982 to 0.992 for `t` values between 50 and 800.
    *   **LSH Performance (char 3-grams)**: Using `r=5, b=32`, the LSH probability for ('D1', 'D2') (similarity 0.978) was 1.0, and for ('D1', 'D3') (similarity 0.5804) it was 0.8869, indicating a high likelihood of collision for similar pairs. Lower similarity pairs like ('D1', 'D4') (0.3051) had a much lower collision probability of 0.0812.

**What were the Jaccard similarities, MinHash approximations, and LSH performance on the MovieLens dataset?**
    *   **Exact Jaccard Similarities**: 10 user pairs had an exact Jaccard similarity $\geq0.5$. When the threshold was raised to $\geq0.8$, there were 0 exact similar pairs.
    *   **MinHash Approximations**: For a similarity threshold of 0.5, MinHash with `t=50` found 0 approximate pairs, resulting in 10 false negatives. With `t=100`, it found 0 pairs and 10 false negatives. With `t=200`, it also found 0 pairs and 10 false negatives. This suggests that `t` values of up to 200 were insufficient to reliably detect pairs with 0.5 similarity using MinHash for this dataset.
    *   **LSH Performance (MovieLens)**:
        *   For (`r=5, b=10, t=50`), LSH generated 0 candidate pairs, leading to 10 false negatives and 0 false positives.
        *   For (`r=5, b=20, t=100`), LSH generated 0 candidate pairs, leading to 10 false negatives and 0 false positives.
        *   For (`r=5, b=40, t=200`), LSH generated 0 candidate pairs, leading to 10 false negatives and 0 false positives.
        *   For (`r=10, b=20, t=200`), LSH generated 0 candidate pairs, leading to 10 false negatives and 0 false positives.
        *   Averaging over 5 runs, the results consistently showed 0 candidate pairs, resulting in average false negatives of 10 and average false positives of 0 for all tested (`r, b, t`) configurations. This indicates that the LSH parameters were not tuned to capture similarities at or above 0.5, or that the `t` value for MinHash signatures was still too low.

### Data Analysis Key Findings

*   **Document Jaccard Similarities**: Character 2-grams and 3-grams showed high similarity between D1 and D2 (0.981 and 0.978 respectively), indicating very similar content. Word 2-grams also confirmed this (0.941). D1 and D3 had moderate character k-gram similarity (e.g., char 3-gram: 0.580), while D1 and D4 showed lower similarity (e.g., char 3-gram: 0.305).
*   **MinHash Effectiveness on Documents**: For document D1 and D2 (exact Jaccard similarity of 0.978 for char 3-grams), MinHash produced highly accurate approximations. For example, `t=600` yielded an approximate similarity of 0.9783. Experiments indicated that `t` values between 50 and 800 provided stable average similarities close to the true value.
*   **LSH Probability on Documents**: The LSH probability function accurately reflected the likelihood of collision based on Jaccard similarity. For example, a pair with 0.978 similarity had a probability of 1.0 of being a candidate for `r=5, b=32`, while a pair with 0.305 similarity had a probability of only 0.0812.
*   **MovieLens Exact Similar Pairs**: A total of 10 user pairs in the MovieLens dataset had a Jaccard similarity $\geq0.5$. However, no user pairs had a Jaccard similarity $\geq0.8$.
*   **MinHash Limitations on MovieLens (with current parameters)**: With `t` values up to 200, MinHash failed to identify any of the 10 exact similar pairs ($\ge0.5$) in the MovieLens dataset, resulting in 100% false negatives (10 out of 10).
*   **LSH Limitations on MovieLens (with current parameters)**: Across all tested LSH configurations (`r`, `b`, `t`), the system consistently reported 0 candidate pairs, leading to 10 false negatives and 0 false positives. This indicates that the chosen parameters or `t` for MinHash signatures were not sufficiently sensitive to detect the existing similar pairs in the MovieLens dataset at the 0.5 similarity threshold.

### Insights or Next Steps

*   The MinHash `t` parameter and LSH `r,b` parameters for the MovieLens dataset require significant tuning. The current configurations were ineffective at identifying similar user pairs, indicating that either the MinHash signatures are not diverse enough (`t` is too low), or the LSH bands and rows are too restrictive, causing all similar pairs to be missed.
*   Future experiments on the MovieLens dataset should focus on increasing `t` for MinHash signature generation and exploring a wider range of `r` and `b` values for LSH, especially those that provide a higher probability of collision for similarities around 0.5. Visualizing the characteristic LSH probability curve for different `r` and `b` would help in selecting optimal parameters.
