# Array Indexing and Slicing in Python

### Syntax

`array[start:stop:step]`

### Steps to extract elements
1. Resolve the `start` and `stop` indices. If negative values are provided, first convert them into positive values by subtracting them from the length of the sequence. This ensures that all index calculations are based on positive values.
   ```
   my_list = [10,20,30,40,50,60,70]
   my_list[-1:-4:1]
   # resolving values for start and stop
   start (-1) = len(my_list) - 1 = 7-1 = 6
   stop  (-4) = len(my_list) - 4 = 7-4 = 3

2. Compare `start`  and `stop` values to identify the step **direction**.
   1. If `start` < `stop`, it means that the stop point is **ahead of us** and to reach it we can move only from left to right (**forward**)
   2. If `start` > `stop`, it means that the stop point is **behind us** and to reach it we can move only from right to left (**backwards**)
3. Based on the above calculated direction, check if `step` is positive or negative
   1. If **direction** is **forward** AND `step` is **negative** then return **empty list**
   2. If **direction** is **backwards** AND `step` is positive then return **empty list**
4. If valid values are found for **direction** and `step`, move accordingly to extract the elements.
   1. If `stop` is **specified** then it is **exclusive**, which means we can actually extract elements **only until the one before the stop index**
   2. If `stop` is **not specified**, then depending on the **direction** (forward or backwards), the **first/last element of the sequence can also be extracted.**

### Overview

Table summarising the various combinations of start, stop, and step values in Python slicing, along with the direction of movement and the resulting values:

| Start Index | Stop Index | Step (Sign) | Start Specified? | Stop Specified? | Direction  | Result             |
|--------------|-------------|-------------|--------------------|-------------------|-------------|----------------------|
| Positive     | Positive     | Positive (+) | Yes               | Yes               | Forward     | Selected Values      |
| Positive     | Positive     | Negative (-) | Yes               | Yes               | Backward    | Empty List (if start >= stop) |
| Positive     | Omitted      | Positive (+) | Yes               | No                | Forward     | Selected Values (to end) |
| Positive     | Omitted      | Negative (-) | Yes               | No                | Backward    | Selected Values (to beginning) |
| Negative     | Positive     | Positive (+) | Yes               | Yes               | Forward     | Selected Values      |
| Negative     | Positive     | Negative (-) | Yes               | Yes               | Backward    | Empty List (if start >= stop) |
| Negative     | Omitted      | Positive (+) | Yes               | No                | Forward     | Selected Values (to end) |
| Negative     | Omitted      | Negative (-) | Yes               | No                | Backward    | Selected Values (to beginning) |
| Omitted      | Positive     | Positive (+) | No                | Yes               | Forward     | Selected Values (from beginning) |
| Omitted      | Positive     | Negative (-) | No                | Yes               | Backward    | Empty List |
| Omitted      | Omitted      | Positive (+) | No                | No                | Forward     | All Values          |
| Omitted      | Omitted      | Negative (-) | No                | No                | Backward    | All Values (Reversed) |
| Any          | Any          | Zero (0)     | -                 | -                 | -            | Empty List          |


**Key:**
* **Start Index / Stop Index:**  Positive, Negative, or Omitted (meaning the default is used).
* **Step (Sign):**  Positive (+) or Negative (-).
* **Start/Stop Specified?:** Yes or No.
* **Direction:** Forward (left to right) or Backward (right to left).
* **Result:** Selected Values (a portion of the sequence) or Empty List (`[]`).
