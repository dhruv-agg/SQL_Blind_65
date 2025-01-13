## Problem Statement

A table named `“famous”` has two columns called `user_id` and `follower_id`.
It represents each user ID has a particular follower ID.
These follower IDs are also users of Facebook / Meta.
Then, find the `famous_percentage` of each user.

> famous_percentage = number of followers a user has / total number of users on the platform.

## 𝐒𝐜𝐡𝐞𝐦𝐚 𝐚𝐧𝐝 𝐃𝐚𝐭𝐚𝐬𝐞𝐭:

```sql
CREATE TABLE famous (user_id INT, follower_id INT);

INSERT INTO famous VALUES
(1, 2), (1, 3), (2, 4), (5, 1), (5, 3),
(11, 7), (12, 8), (13, 5), (13, 10),
(14, 12), (14, 3), (15, 14), (15, 13);
```

| **user_id** | **follower_id** |
| ----------- | --------------- |
| 1           | 2               |
| 1           | 3               |
| 2           | 4               |
| 5           | 1               |
| 5           | 3               |
| 11          | 7               |
| 12          | 8               |
| 13          | 5               |
| 13          | 10              |
| 14          | 12              |
| 14          | 3               |
| 15          | 14              |
| 15          | 13              |

## Solution

```sql
WITH distinct_users AS (
    SELECT user_id FROM famous
    UNION
    SELECT follower_id FROM famous
),
followers_count AS (
    SELECT user_id, COUNT(follower_id) AS followers
    FROM famous
    GROUP BY user_id
)
SELECT f.user_id, f.followers, round((f.followers * 100.0) / (SELECT count(*) from distinct_users),2) as famous_percentage
 FROM followers_count as f;
```

## 𝐄𝐱𝐩𝐥𝐚𝐧𝐚𝐭𝐢𝐨𝐧 𝐨𝐟 𝐭𝐡𝐞 𝐐𝐮𝐞𝐫𝐲:

1. **`distinct_users` CTE**:
   Combines `user_id` and `follower_id` using `UNION` to get all unique users on the platform. This helps us determine the total number of users.

2. **`follower_count` CTE**:
   Counts the number of followers for each `user_id` by grouping the rows in the famous table. This gives a list of users with their follower counts.

3. **Final `SELECT` Statement**:
   Uses the data from `follower_count` and `distinct_users` to calculate the `famous_percentage` for each user.

### Output

| **user_id** | **followers** | **famous_percentage** |
| ----------- | ------------- | --------------------- |
| 1           | 2             | 15.38                 |
| 2           | 1             | 7.69                  |
| 5           | 2             | 15.38                 |
| 11          | 1             | 7.69                  |
| 12          | 1             | 7.69                  |
| 13          | 2             | 15.38                 |
| 14          | 2             | 15.38                 |
| 15          | 2             | 15.38                 |
