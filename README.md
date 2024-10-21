# Robotics Computer Vision Developer Assessment 
<p style="text-align:center;">Done by: <a href="https://www.linkedin.com/in/amer-ghazal/">Amer Ghazal</a></p>

Below you will find the solutions for the questions I received as an assessment from Microvia Hiring Team
([PDF](questions.pdf)) attatched.


## Question 1

```cpp
#include <iostream>
#include <vector>

using namespace std;

int countJumpSequences(int N, int K, int L, int M, vector<int>& memo) {
    // Base case: If the distance is 0, there's one sequence (no jumps)
    if (N == 0) {
        return 1;
    }

    // If the distance is negative, no valid sequence exists
    if (N < 0) {
        return 0;
    }

    // If the result for this distance is already memoized, return it
    if (memo[N] != -1) {
        return memo[N];
    }

    // Calculate the number of sequences for each jump option
    int sequencesK = countJumpSequences(N - K, K, L, M, memo);
    int sequencesL = countJumpSequences(N - L, K, L, M, memo);
    int sequencesM = countJumpSequences(N - M, K, L, M, memo);

    // Total sequences is the sum of sequences for each jump option
    int totalSequences = sequencesK + sequencesL + sequencesM;

    // Memoize the result for this distance
    memo[N] = totalSequences;

    return totalSequences;
}

int main() {
    int K = 2;
    int L = 3;
    int M = 5;
    int N = 5;

    // Initialize memoization table with -1 (indicating not calculated)
    vector<int> memo(N + 1, -1);

    int numSequences = countJumpSequences(N, K, L, M, memo);
    cout << numSequences << endl; // Output: 3

    return 0;
}
```


## Question 2: Code Analysis and Correction

**Errors and Fixes:**

1.  **Case Sensitivity:** `Result[i]` should be `result[i]`. C++ is case-sensitive.
2.  **Memory Allocation:** The `result` array should be dynamically allocated using `new int[size]` to prevent it from being deallocated when the function returns.
3.  **Error Handling:** Division by zero should be handled.

**Corrected Code:**

```cpp
#include <cstddef> 

int *div(int *data, size_t size, int div) {
    if (div == 0) {
        return nullptr; // Handle division by zero
    }

    int *result = new int[size]; 
    for (size_t i = 0; i < size; ++i) {
        result[i] = data[i] / div;
    }
    return result;
}
```

## Question 3: Convolutional Neural Network

1. Number of Variable Parameters:

    Each filter has 5 * 5 = 25 weights + 1 bias = 26 parameters.
    Total parameters: 26 parameters/filter * 6 filters = </br>
    **156 parameters**

2. Number of Feature Maps in the Output:

    The output has the same number of feature maps as filters: 
    </br>
    **6 feature maps**

3. Resolution of the Output Feature Map:

    Output size = (Input size + 2 * Padding - Filter size) / Stride + 1
    Output size = (55 + 2 * 2 - 5) / 3 + 1 = 19 </br>
    **Resolution:** 19 * 19

## Question 4: Straight Line Detection

### Hough Transform:

    Edge Detection: Apply an edge detector (e.g., Canny, Sobel).
    Hough Space: Create a parameter space (ρ, θ) for lines.
    Voting: For each edge point, increment cells in Hough space corresponding to lines passing through it.
    Peak Detection: Find cells with the most votes (lines).
    Line Extraction: Convert (ρ, θ) back to line equations.

### Alternative Methods:

    RANSAC: Robustly fits lines to data with outliers.
    LSD: Directly extracts line segments.

## Question 5: World Coordinates to Pixel Coordinates

World to Camera Coordinates: Transform using camera's extrinsic parameters (rotation and translation):

    [X_c, Y_c, Z_c]^T = R * [X_w, Y_w, Z_w]^T + t

Projection: Project 3D camera coordinates to 2D image plane using intrinsic parameters (focal length, principal point):

    u = f * (X_c / Z_c) + u_0
    v = f * (Y_c / Z_c) + v_0

Pixel Coordinates: Convert to pixel coordinates considering pixel size and skew:

    u_p = s_x * u + α * v
    v_p = s_y * v

## Question 6: Pinhole Camera Geometry

1. Projection of Camera B in Image Area A:

    Camera B projects onto the left side of Area A (cells A1, A2, A5, A6).

2. Projection of Camera A in Image Area B:

    Camera A projects onto the back of Area B (cells B3, B4).

3. Points of Camera B Projected onto Area A:

    All 8 vertices of Camera B are projected.

4. Points of Camera A Projected onto Area B:

    Only the 4 back vertices of Camera A are projected.

5. XYZ Coordinates of Camera A's Pinhole in Area B:

    Find the intersection of the line through (8, 1, 2) and (1, 7, 1) with the plane x = 0.
    Approximate coordinates: (0, 7.67, 1.67)

6. XYZ Coordinates of Camera B's Pinhole in Area A:

    Find the intersection of the line through (1, 7, 1) and (8, 1, 2) with the plane y = 0.
    Approximate coordinates: (8.67, 0, 1.33)

<br>
<br>
<br>
Thanks for reading! 😊
