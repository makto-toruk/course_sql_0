# 11. Views and Subselects

## 11.1 Simple Subselect

```sql
-- world.db
CREATE TABLE t ( a TEXT, b TEXT );
INSERT INTO t VALUES ( 'NY0123', 'US4567' );
INSERT INTO t VALUES ( 'AZ9437', 'GB1234' );
INSERT INTO t VALUES ( 'CA1279', 'FR5678' );
SELECT * FROM t;
```

```sql
SELECT SUBSTR(a, 1, 2) AS State, SUBSTR(a, 3) AS SCode, 
SUBSTR(b, 1, 2) AS Country, SUBSTR(b, 3) AS CCode FROM t;
```
- This is a straight statement. It uses substrings to list table information in more columns

- Here's the interesting part. We can use the above as a datasource to SELECT from, making it a SUBSELECT. Example:

```sql
SELECT co.Name, ss.CCode FROM (
    SELECT SUBSTR(a, 1, 2) AS State, SUBSTR(a, 3) AS SCode,
      SUBSTR(b, 1, 2) AS Country, SUBSTR(b, 3) AS CCode FROM t
  ) AS ss
  JOIN Country AS co
    ON co.Code2 = ss.Country
;
```
- This seems like it could be really useful for my work.

## 11.2 searching within a result set
-- album.db

```sql
SELECT DISTINCT album_id FROM track WHERE duration <= 90;
```
- returns album IDs. But what albums are they?

```sql
SELECT * FROM album
  WHERE id IN (SELECT DISTINCT album_id FROM track WHERE duration <= 90)
;
```

- To get more details about all the the tracks in these albums:

```sql
SELECT a.title AS album, a.artist, t.track_number AS seq, t.title, t.duration AS secs
  FROM album AS a
  JOIN track AS t
    ON t.album_id = a.id
  WHERE a.id IN (SELECT DISTINCT album_id FROM track WHERE duration <= 90)
  ORDER BY a.title, t.track_number
;
```

- What if we want only those two tracks?
    - JOIN on the subselect
    
```sql
SELECT a.title AS album, a.artist, t.track_number AS seq, t.title, t.duration AS secs
  FROM album AS a
  JOIN (
    SELECT DISTINCT album_id, track_number, duration, title
      FROM track
      WHERE duration <= 90
  ) AS t
    ON t.album_id = a.id
  ORDER BY a.title, t.track_number
;
```

## 11.3 Creating a view
-- album.db

A view is effectively a saved table.
```sql
SELECT id, album_id, title, track_number, 
  duration / 60 AS m, duration % 60 AS s FROM track;
  ```

- A view can be saved in the database instead of creating a new table.
```sql
CREATE VIEW trackView AS
  SELECT id, album_id, title, track_number, 
    duration / 60 AS m, duration % 60 AS s FROM track;
```

- When you're done, drop the view as: `DROP VIEW IF EXISTS trackView;`

## 11.4 Joined view
-- album.db

```sql
SELECT a.artist AS artist,
    a.title AS album,
    t.title AS track,
    t.track_number AS trackno,
    t.duration / 60 AS m,
    t.duration % 60 AS s
  FROM track AS t
    JOIN album AS a
      ON a.id = t.album_id
;
```
- Select always returns a table

```sql
CREATE VIEW joinedAlbum AS
  SELECT a.artist AS artist,
      a.title AS album,
      t.title AS track,
      t.track_number AS trackno,
      t.duration / 60 AS m,
      t.duration % 60 AS s
    FROM track AS t
    JOIN album AS a
      ON a.id = t.album_id
;
```
- That table can be saved as a view.

```sql
SELECT artist, album, track, trackno, 
   m || ':' || substr('00' || s, -2, 2) AS duration
    FROM joinedAlbum;
```
- and then a complex query can be run on that view.
- recall `||` is concatenation in SQLite
- `00` is just a trick to convert into format mm:ss