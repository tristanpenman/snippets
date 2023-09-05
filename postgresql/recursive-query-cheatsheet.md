
# Recursive Query Cheatsheet

## Problem

Say you have the following table in a PostgreSQL database:

    CREATE TABLE posts(
      id INT NOT NULL PRIMARY KEY,
      content TEXT NOT NULL,
      parent_id INT,
      CONSTRAINT fk_parent FOREIGN KEY(parent_id) REFERENCES posts(id));

This defines a table of posts, each of which has an optional reference to a 'parent' post.

How would we fetch all of the descendents of a given post?

## Solution

We can use a recursive query!

Let's start with some example data:

    INSERT INTO posts(id, content, parent_id) VALUES
      (1, 'A', NULL),
      (2, 'B', 1),
      (3, 'C1', 2),
      (4, 'C2', 2),
      (5, 'D', NULL),
      (6, 'E', 5);

This represents two threads of posts.

We can get a post and its descendents using a query like this:

    WITH RECURSIVE post_descendents(id, content, parent_id, depth) AS (
      SELECT posts.id, posts.content, posts.parent_id, 0
        FROM posts
        WHERE id = 1
      UNION ALL
        SELECT posts.id, posts.content, posts.parent_id, post_descendents.depth + 1
        FROM posts, post_descendents
        WHERE posts.parent_id = post_descendents.id
    ) SELECT * FROM post_descendents;

Resulting in:

     id | content | parent_id | depth
    ----+---------+-----------+-------
      1 | A       |           |     0
      2 | B       |         1 |     1
      3 | C1      |         2 |     2
      4 | C2      |         2 |     2
    (4 rows)

This works even if the starting record is not at the root (here we start at post 2):

    WITH RECURSIVE post_descendents(id, content, parent_id, depth) AS (
      SELECT posts.id, posts.content, posts.parent_id, 0
        FROM posts
        WHERE id = 2
      UNION ALL
        SELECT posts.id, posts.content, posts.parent_id, post_descendents.depth + 1
        FROM posts, post_descendents
        WHERE posts.parent_id = post_descendents.id
    ) SELECT * FROM post_descendents;

Resulting in:

     id | content | parent_id | depth
    ----+---------+-----------+-------
      2 | B       |         1 |     0
      3 | C1      |         2 |     1
      4 | C2      |         2 |     1
    (3 rows)
