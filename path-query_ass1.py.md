# Path-query-ass1

## Path Parameters:
1. what are path parameters in FastAPI?

    Path parameters are variables used in the URL path of a web service or API endpoint to define specific resources or entities. In RESTful APIs, they are part of the URL structure and are used to specify unique identifiers or attributes associated with the requested resource.

    Path parameters can be declared in FastAPI  using the smae syntax used by python format strings.

e.g 
''' 
    python
    from fastapi import FastAPI

    app = FastAPI()


    @app.get("/items/{user_id}")
    async def read_users(user_id):
        return {"user_id": user_id}

'''
    In the above example user-id is a path parameter, the value of the path parameter user_id will be passed to your function as the argument User_id.

2. How are path parameters defined in FastAPI route declarations?

    Path parameters can be declared in FastAPI  using the same syntax used by python format strings.

e.g 
''

    from fastapi import FastAPI

    app = FastAPI()


    @app.get("/items/{user_id}")
    async def read_users(user_id):
        return {"user_id": user_id}

'''
    In the above example user-id is a path parameter, the value of the path parameter user_id will be passed to your function as the argument User_id.

3. Can path parameters have default values? If yes, how can they be set?
    YES, In Fast API path parameters can have default values but however it does not change anything as path parameters will always be required
    how to set default value :

'''


    @app.put("/items/{item_id}")
    async def root(item_id: str = "me"):
        return {"item_id": item_id}

'''
4. Provide an example of a FastAPI route with path parameters.
'''

    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/items/{item_id}/detail/{detail_id}")
    async def read_item_detail(item_id: int, detail_id: str):
        return {"item_id": item_id, "detail_id": detail_id}
'''
You can test this endpoint by making requests to URLs like
 [http://localhost:8000/items/1/detail/foo],
 [http://localhost:8000/items/2/detail/bar]


## Query Parameters:

1.  What are query parameters in FastAPI?
Query parameters are functions parameters not declared in the path as path paramete, so Fast API will interpret them as Query parameter.
for example:

'''

    from fastapi import FastAPI

    app = FastAPI()

    fake_name_db = [{"name_1": "Tinubu"}, {"name_2": "Buhari"}, {"name_3": "Abacha"}]


    @app.get("/names/")
    async def read_name(skip: int = 0, limit: int = 10):
        return fake_name_db[skip : skip + limit]


'''

    in the example Skip and Limit are Query parameters, the request URL will look like this 
    [http://127.0.0.1:8000/names/?skip=0&limit=10]

2.  How are query parameters defined in FastAPI route declarations?
In FastAPI, query parameters are defined as function parameters in the route declaration using Python's type annotations. 

'''
    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/items/")
    async def read_items(category: str, page: int = 1):
        return {"category": category, "page": page}

'''
    in the above practical example category and page are query parameters with annotations(Typing) to specify there data types, page is specified as integer with a default value of 1.

3. What is the difference between path parameters and query parameters?
    1. path parameters are defined both in the path and in the function, but quetry parameters are define as function parametes only.

    2. path parameters can take default value , how ever it does nothing as path parameter will always be required, but query parameter can take defualt value that will work when a value is not provider by the client.

    3. path parameter is used to get to a particular resource but query parameter is used to get a specific item or section of a resource.

4. Can query parameters have default values? If yes, how can they be set?
Yes , Query parameter can have default value.

example:
'''
    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/items/")
    async def read_items(category: str, page: int = 1):
        return {"category": category, "page": page}

'''
in the example above the Query parameter "page" have a default value of 1, in a situation where we have several category pages , if a user wants to get to the category page but does not specify which category page, the server will reture the first page .

5. Provide an example of a FastAPI route with query parameters

''' python

    from fastapi import FastAPI

    app = FastAPI()

    fake_name_db = [{"name_1": "Tinubu"}, {"name_2": "Buhari"}, {"name_3": "Abacha"}]


    @app.get("/names/")
    async def read_name(skip: int = 0, limit: int = 10):
        return fake_name_db[skip : skip + limit]
'''

## Combining Path and Query Parameters:

1. Can a FastAPI route have both path parameters and query parameters? If yes,
provide an example.
yes FastAPI route can have both path parameters and query parameters FastAPI can identify which is which.

'''
    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/users/{user_id}/items/{item_id}")
    async def read_user_item(
        user_id: int, item_id: str, q: str | None = None, short: bool = False
    ):
        item = {"item_id": item_id, "owner_id": user_id}
        
        if q:
            # If 'q' is provided, add it to the item dictionary
            item.update({"q": q})
        
        if not short:
            item.update({"description": "This is an amazing item that has a long description"})
        return item
'''

    In the example above user_id and item_id are both path Parameters while q and short are Query parameters

2. What is the order of precedence if a parameter is defined both in the path and as
a query parameter?

    In FastAPI, if a parameter is defined both in the path and as a query parameter, the value from the path parameter takes precedence over the query parameter.




## Data Types and Validation:

1. How does FastAPI handle data types and validation for path and query
parameters?

 for path parameters ,when the types is specified in the route to be  either a string , int or any other type ,FastAPI provides validation, if a client should input the wrong type for the path parameter , Fast API will return an error message "Please correct the following validation errors and try again".

    for Query parameter, same as above applies , additionally you can make a path parameter required or not , and Fast API will handle when the type of Query parameter is given as None.

Generaly by using Annotations and typing Fast API will provide validation.


2. What are some common data types that can be used for path and query
parameters?
    some common type are string (str), integres (int) and bool ( boolean value true or false )


3. How can you enforce validation rules on path and query parameters in FastAPI?
    1. by using annotations and typings

    2. Define Pydantic Models: Create Pydantic models that represent the expected structure of your path and query parameters. Add validation rules using Pydantic's field types and validation functions.

    3. Use Models in Route Parameters: In your route declaration, use the Pydantic models as function parameters. FastAPI will automatically validate incoming request data against the specified models.

    4. Handle Validation Errors: If validation fails, FastAPI will automatically return an HTTP 422 Unprocessable Entity error response with details about the validation errors. You can customize error handling and responses as needed.


## Usage and Benefits:

1. In what scenarios would you use path parameters over query parameters, and
vice versa?

    we use path parameters to identify a specific resource or a particular resource but query parameters are used identify a part or section of a particular resource, take for instance the example below.

    Suppose you are implementing RESTful API endpoints for an entity called Car. You would structure your endpoints like this:

    GET /cars
    GET /cars/:id
    POST /cars
    PUT /cars/:id
    DELETE /cars/:id

    This way you are only using path parameters when you are specifying which resource to fetch, but this does not sort/filter the resources in any way.

    Now suppose you wanted to add the capability to filter the cars by color in your GET requests. Because color is not a resource (it is a property of a resource), you could add a query parameter that does this. You would add that query parameter to your GET /cars request like this:

    GET /cars?color=blue

    This endpoint would be implemented so that only blue cars would be returned.

    We can add more filtering parameters using the & symbol:

    GET /cars?color=blue&brand=ferrari

    As far as syntax is concerned, your URL names should be all lowercase. 

2. What are the benefits of using path and query parameters in API design?


3. Can you provide examples of real-world use cases where path and query
parameters would be employed effectively?

    ### Path Parameters:

    #### Expressiveness: Path parameters allow for expressive and intuitive URLs. They make endpoints more human-readable and logical. For instance, /users/{user_id} clearly indicates that we're operating on a specific user.

    #### Strong Typing: FastAPI leverages Python's type hints to enforce strong typing. This ensures that path parameters are of the expected data type, reducing errors and enhancing code reliability.

    #### Validation: FastAPI automatically validates path parameters based on their declared types. This built-in validation prevents malformed requests from reaching the application logic, improving overall robustness.

    #### Documentation: Path parameters are automatically documented in the API documentation generated by FastAPI. Developers can quickly understand the expected parameters and their types, streamlining the development process.

    ### Query Parameters:
    Flexibility: Query parameters provide flexibility in API requests by allowing optional parameters. Clients can tailor their requests based on specific needs without altering the URL structure. For instance, /users?status=active can be extended with additional query parameters like page, limit, etc., as needed.

    #### Filtering and Sorting: Query parameters are commonly used for filtering and sorting data. They enable clients to specify criteria for retrieving resources, such as filtering users by status or sorting products by price.

    #### Pagination: Query parameters are instrumental in implementing pagination mechanisms. Clients can request a subset of resources by specifying page and limit parameters, enhancing performance and scalability for large datasets.

    #### Bookmarking: Query parameters facilitate bookmarking of specific views or searches. Users can save URLs with query parameters to revisit or share particular states of the application easily.


## Error Handling:

1. How does FastAPI handle errors related to missing or invalid path/query
parameters?

    Errors can be handle gracefully be raising a HTTPException, although
    Fast API has its own default ways of handling errors by displaying the regulary error codes e.g 404 (400 to 499 are client side error code)

    In this example, when the client requests an item by an ID that doesn't exist, raise an exception with a status code of 404

'''
    from fastapi import FastAPI, HTTPException

    app = FastAPI()

    items = {"foo": "The Foo Wrestlers"}


    @app.get("/items/{item_id}")
    async def read_item(item_id: str):
        if item_id not in items:
            raise HTTPException(status_code=404, detail="Item not found")
        return {"item": items[item_id]}
        

'''
    If the client requests http://example.com/items/foo (an item_id "foo"), that client will receive an HTTP status code of 200, and a JSON response of:

 '''
    {
    "item": "The Foo Wrestlers"
    }
 '''

    If the client requests http://example.com/items/foo (an item_id "foo"), that client will receive an HTTP status code of 200, and a JSON response of:


    {
    "item": "The Foo Wrestlers"
    }

    other ways of handling error involves:
    1. Adding custom headers
    2. installing custom exception handlers 

2. Can you customize error responses for cases where required parameters are
missing or validation fails?

    the example above clearly illustrates customizing error message, we paased a message "item not found " in the form of a JSON .

    '''
    {
        "detail": "Item not found"
    }
    '''


    When raising an HTTPException, you can pass any value that can be converted to JSON as the parameter detail, not only str.

    You could pass a dict, a list, etc.

    They are handled automatically by FastAPI and converted to JSON.
