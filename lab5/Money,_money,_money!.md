# Challenge: Money, money, money!  Writeup

- **Vulnerability**: SQL Injection
  - _Eg, Manipulating the SQL query for updating a user's profile bio to also update their token balance._
- **Where**: The vulnerability is present in the profile bio update feature.
  - _Eg, A form or API endpoint that allows users to update their profile bio._
- **Impact**: Exploiting this vulnerability allows an attacker to alter their token balance.
  - _Eg, This can lead to unauthorized modification of user tokens, potentially affecting account balances or privileges._

- **NOTE**: This vulnerability arises from improper handling of user input in SQL queries, allowing an attacker to inject additional SQL commands.

## Steps to reproduce

1. Access the profile bio update feature.
2. In the bio input field, enter the following payload: `hey', Tokens = '10093`
3. Submit the update request.
4. The backend SQL query, which might be structured as `UPDATE user SET bio = '[user_input]' WHERE userid = [current_user_id]`, is manipulated.
5. The injected query becomes: `UPDATE user SET bio = 'hey', Tokens = '10093' WHERE userid = [current_user_id]`.
6. This query not only updates the bio to "hey" but also sets the `Tokens` column to 10093 for the current user.
7. As a result, the attacker can arbitrarily change their token balance.
