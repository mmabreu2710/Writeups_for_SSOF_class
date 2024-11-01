# Challenge: Wow,_it_can't_be_more_juicy_than_this! Writeup

- **Vulnerability**: SQL Injection
  - _Eg, Exploiting the search feature to execute unauthorized SQL commands, including accessing database schema information and hidden data._
- **Where**: The vulnerability is present in the search input field of the website.
  - _Eg, A search box on a blog or content site that queries the database for user-supplied terms._
- **Impact**: Discovery of database schema and unauthorized access to hidden or sensitive data.
  - _Eg, Accessing the `sqlite_master` table to reveal database structure and subsequently accessing a secret blog post table, potentially exposing confidential information._

- **NOTE**: This vulnerability occurs due to the website's failure to properly sanitize user input in the search functionality, allowing the execution of arbitrary SQL commands.

## Steps to reproduce

### Discovering Database Structure

1. Navigate to the search feature on the website.
2. In the search input, enter the SQL injection payload to access database schema: `' UNION SELECT name, tbl_name, sql FROM sqlite_master --`
3. Submit the search.
4. The backend SQL query, originally meant to search blog posts, is manipulated:

    ```sql
    SELECT id, title, content FROM blog_post WHERE title LIKE '%' ' UNION SELECT name, tbl_name, sql FROM sqlite_master -- '%' OR content LIKE '%' ' UNION SELECT name, tbl_name, sql FROM sqlite_master -- '%'
    ```

    The search results now include entries from the `sqlite_master` table, revealing the structure of the database, including table names and their creation SQL.

### Accessing Hidden Data

6. After discovering the `secret_blog_post` table from the `sqlite_master` information, use a similar injection targeting this table.
7. Enter the payload: `' UNION SELECT id, title, content FROM secret_blog_post --`
8. This reveals entries from the `secret_blog_post` table, leaking potentially sensitive or hidden information.
