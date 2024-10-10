# Project Walkthrough: Sales Data REST API

## Overview

This project will demonstrate how to create a simple REST API to manage sales data, allowing you to perform CRUD (Create, Read, Update, Delete) operations. This API can be useful for a Business Intelligence Developer looking to analyze sales data effectively.

## Prerequisites

- Basic knowledge of Python
- Python installed on your machine
- Familiarity with using the command line

## Step 1: Set Up Your Environment

1. **Install Flask**:
   Open your terminal and run:
   ```bash
   pip install Flask
   ```

2. **Create a Project Directory:**

```bash
mkdir sales_api
cd sales_api
```

3. **Create the Main Application File with Python:**

```bash
python -c "open('app.py', 'w').close()"
```

## Step 2: Build the API
Open app.py in your text editor and add the following code:
- NOTE: Explanation of full code to follow

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

# Sample sales data
sales_data = [
    {"id": 1, "product": "Laptop", "quantity": 10, "price": 1200},
    {"id": 2, "product": "Mouse", "quantity": 25, "price": 25},
]

# GET all sales records
@app.route('/sales', methods=['GET'])
def get_sales():
    return jsonify(sales_data)

# GET a single sales record by ID
@app.route('/sales/<int:sale_id>', methods=['GET'])
def get_sale(sale_id):
    sale = next((item for item in sales_data if item["id"] == sale_id), None)
    return jsonify(sale) if sale else ('', 404)

# POST a new sales record
@app.route('/sales', methods=['POST'])
def create_sale():
    new_sale = request.get_json()
    sales_data.append(new_sale)
    return jsonify(new_sale), 201

# PUT to update a sales record
@app.route('/sales/<int:sale_id>', methods=['PUT'])
def update_sale(sale_id):
    sale = next((item for item in sales_data if item["id"] == sale_id), None)
    if sale:
        updated_data = request.get_json()
        sale.update(updated_data)
        return jsonify(sale)
    return ('', 404)

# DELETE a sales record
@app.route('/sales/<int:sale_id>', methods=['DELETE'])
def delete_sale(sale_id):
    global sales_data
    sales_data = [item for item in sales_data if item["id"] != sale_id]
    return ('', 204)

if __name__ == '__main__':
    app.run(debug=True)

```

### Explanation of Step 2
1. Importing Necessary Modules:

```python
from flask import Flask, jsonify, request
```

- Flask: The core class used to create a Flask application. It provides the essential methods and properties needed to build the web application.
- jsonify: Converts Python dictionaries or lists into JSON format, the standard format for API responses. It also sets the appropriate Content-Type header (application/json).
- request: Represents the incoming HTTP request, containing all the data sent by the client, including URL parameters, form data, and JSON payloads.

2. Creating the Flask App:

```python
app = Flask(__name__)
```
- Initializes a new Flask application instance. The __name__ variable tells Flask where to look for resources like templates and static files.

3. Sample Sales Data:

```python
sales_data = [
    {"id": 1, "product": "Laptop", "quantity": 10, "price": 1200},
    {"id": 2, "product": "Mouse", "quantity": 25, "price": 25},
]
```
- A list called sales_data holds our sample sales records, represented as dictionaries. This acts as a placeholder for a database in this simple example.

### Defining API Endpoints
Next, we define the API endpoints that allow users to interact with the sales data.

1. GET All Sales Records:

```python
@app.route('/sales', methods=['GET'])
def get_sales():
    return jsonify(sales_data)
```
- @app.route('/sales', methods=['GET']): This decorator creates a new route. When a client makes a GET request to /sales, Flask calls the get_sales function.
- def get_sales():: This function handles the incoming request.
- return jsonify(sales_data): Converts the sales_data list to JSON format and returns it as the response. This allows clients to retrieve all sales records.

2. GET a Single Sales Record by ID:

```python
@app.route('/sales/<int:sale_id>', methods=['GET'])
def get_sale(sale_id):
    sale = next((item for item in sales_data if item["id"] == sale_id), None)
    return jsonify(sale) if sale else ('', 404)
```
- /<int:sale_id>: Captures an integer from the URL as sale_id. Flask converts it to an integer before passing it to the function.
- def get_sale(sale_id):: The function to handle requests for a specific sale.
- next((item for item in sales_data if item["id"] == sale_id), None): Searches for the first item in sales_data where the id matches sale_id. If found, it assigns that item to sale, otherwise assigns None.
- return jsonify(sale) if sale else ('', 404): If a sale is found, it returns the sale as JSON; if not, it returns a 404 error indicating that the resource was not found.

3. POST a New Sales Record:

```python
@app.route('/sales', methods=['POST'])
def create_sale():
    new_sale = request.get_json()
    sales_data.append(new_sale)
    return jsonify(new_sale), 201
```
- methods=['POST']: Specifies that this endpoint will handle POST requests, typically used to create new resources.
- def create_sale():: The function to handle the creation of a new sale.
- new_sale = request.get_json(): Retrieves the JSON payload from the incoming request and converts it to a Python dictionary.
- sales_data.append(new_sale): Adds the new sale to the sales_data list.
- return jsonify(new_sale), 201: Returns the newly created sale and a 201 status code, indicating that a new resource has been successfully created.

4. PUT to Update a Sales Record:

```python
@app.route('/sales/<int:sale_id>', methods=['PUT'])
def update_sale(sale_id):
    sale = next((item for item in sales_data if item["id"] == sale_id), None)
    if sale:
        updated_data = request.get_json()
        sale.update(updated_data)
        return jsonify(sale)
    return ('', 404)
```
- methods=['PUT']: Indicates that this endpoint will handle PUT requests, typically used for updating existing resources.
- def update_sale(sale_id):: The function to handle updates to a specific sale.
- if sale:: Checks if the sale was found.
- updated_data = request.get_json(): Retrieves the updated data from the request.
- sale.update(updated_data): Updates the found sale with the new data.
- return jsonify(sale): Returns the updated sale as a JSON response.

5. DELETE a Sales Record:

```python
@app.route('/sales/<int:sale_id>', methods=['DELETE'])
def delete_sale(sale_id):
    global sales_data
    sales_data = [item for item in sales_data if item["id"] != sale_id]
    return ('', 204)
```
- methods=['DELETE']: Indicates that this endpoint will handle DELETE requests, used to remove resources.
- def delete_sale(sale_id):: The function to handle deletion of a specific sale.
- global sales_data: Allows modification of the sales_data variable defined outside this function.
- sales_data = [item for item in sales_data if item["id"] != sale_id]: Creates a new list that excludes the sale with the specified ID.
- return ('', 204): Returns a 204 status code indicating that the deletion was successful and there is no content to return.

# Step 3: Run the API
1. In your terminal, navigate to the project directory if you’re not already there.
2. Run the application:
```bash
python app.py
```

# Step 4: Test the API
You can use tools like Postman or cURL to test the API. Here are some example requests:

1. GET All Sales:

```bash
curl http://127.0.0.1:5000/sales
```

2. GET Sale by ID:

```bash
curl http://127.0.0.1:5000/sales/1
```

3. POST a New Sale:

```bash
curl -X POST -H "Content-Type: application/json" -d '{"id": 3, "product": "Keyboard", "quantity": 15, "price": 50}' http://127.0.0.1:5000/sales
```

4. PUT Update Sale:

```bash
curl -X PUT -H "Content-Type: application/json" -d '{"price": 55}' http://127.0.0.1:5000/sales/3
```

5. DELETE a Sale:

```bash
curl -X DELETE http://127.0.0.1:5000/sales/3
```


# Best Practices
1. Use JSON:
   JSON is the standard format for APIs because it’s lightweight and easy to work with. Using jsonify helps ensure that your responses are properly formatted.

2. Status Codes:
   Always return appropriate HTTP status codes:
  - 200 OK for successful GET requests.
  - 201 Created for successful POST requests.
  - 204 No Content for successful DELETE requests.
  - 404 Not Found for requests that fail to find the specified resource.

3. Modular Code:
   For larger applications, consider separating your routes, logic, and data management into different files or modules. This enhances maintainability.

5. Error Handling:
   In production applications, implement more robust error handling to catch and respond to exceptions gracefully.

7. Data Validation:
   Validate incoming data to ensure it meets your API's requirements before processing it.

# Conclusion
You've built a basic REST API to manage sales data. This project gives you foundational knowledge of REST principles, Python programming, and the Flask framework. Understanding these concepts is essential for a Business Intelligence Developer working with APIs to analyze data effectively.
