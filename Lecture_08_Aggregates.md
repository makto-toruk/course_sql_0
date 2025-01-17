# 8. Aggregates

## 8.1 Aggregate Data
```sql
-- world.db
```
- `SELECT COUNT(*) FROM Country;`
    - `COUNT` is an aggregate function
- `GROUP BY` groups the results before calling `COUNT`
  ```sql
  SELECT Region, COUNT(*)
    FROM Country
    GROUP BY Region
  ;
  ```
- This also works with `JOIN` queries
  ```sql
  -- album.db
  SELECT a.title AS Album, COUNT(t.track_number) as Tracks
    FROM track AS t
    JOIN album AS a
      ON a.id = t.album_id
    GROUP BY a.id
    ORDER BY Tracks DESC, Album
  ;
  ```
- to show only albums that have greater than 10 tracks, use a `HAVING` clause along with aggregation
  ```sql
  SELECT a.title AS Album, COUNT(t.track_number) as Tracks
    FROM track AS t
    JOIN album AS a
      ON a.id = t.album_id
    GROUP BY a.id
    HAVING Tracks >= 10
    ORDER BY Tracks DESC, Album
  ;
  ```
- to show albums by a particular band, use a `WHERE` clause
  ```sql
  SELECT a.title AS Album, COUNT(t.track_number) as Tracks
    FROM track AS t
    JOIN album AS a
      ON a.id = t.album_id
    WHERE a.artist = 'The Beatles'
    GROUP BY a.id
    HAVING Tracks >= 10
    ORDER BY Tracks DESC, Album
  ;
  ```
- `WHERE` clause must be before `GROUP BY` because `WHERE` acts before aggregation
- `HAVING` after `GROUP BY` since it acts on aggregate data
- `HAVING`: aggregate data
- `WHERE`: non-aggregate data

## 8.2 Aggregate functions
```sql
-- world.db
```
- `SELECT COUNT(*) FROM Country;`
    - counts all rows in the table
- `SELECT COUNT(Population) FROM Country;`
    - if a column is specified, number of non-NULL rows are returned
- `SELECT AVG(Population) FROM Country;`
    - `AVG` returns the average value of that column
- `SELECT Region, AVG(Population) FROM Country GROUP BY Region;`
    - `GROUP BY` to return `AVG` of each region
- `SELECT Region, MIN(Population), MAX(Population) FROM Country GROUP BY Region;`
    - `MIN`, `MAX`
- `SELECT Region, SUM(Population) FROM Country GROUP BY Region;`
    - `SUM`

## 8.3 DISTINCT Aggregates
- `SELECT COUNT(HeadOfState) FROM Country;`
    - rows in Country that have a value for HeadOfState
- `SELECT HeadOfState FROM Country ORDER BY HeadOfState;`
    - sort. notice many rows have the same head of state
- `SELECT COUNT(DISTINCT HeadOfState) FROM Country;`
    - to remove duplicate, use `DISTINCT` to count unique values
    - older versions don't support `DISTINCT`. some versions use keyword `UNIQUE`