# Best Practices for REST APIs

These best practices will guide you in designing and building a well-structured and efficient RESTful API. Following these guidelines will help create a more intuitive and maintainable API for your projects.

## 1. Organize around Resources

- Identify and focus on the primary business entities that your API exposes (e.g., customers, orders).
- Use plural form of nouns for resource URIs (e.g., `/customers`) instead of verbs (e.g., `/create-customer`).
- Group related entities into collections (e.g., `/customers`) with individual item URIs (e.g., `/customers/{id}`).
- Adopt a consistent naming convention for URIs.

### Enhancing Resource Security

- Use Random IDs (UUID/GUID): Consider using randomly generated IDs (such as UUIDs or GUIDs) for resource identification instead of predictable numeric or sequential IDs. This practice helps prevent resource guessing, enhancing security by making it more difficult for malicious actors to enumerate or guess resource URIs. Random IDs can be challenging to predict and provide an additional layer of security.

## 2. Define API Operations with HTTP Methods

- Use standard HTTP methods:
    - `GET` for retrieving resource representations.
    - `POST` for creating new resources or triggering operations.
    - `PUT` for updating resources.
    - `PATCH` for partial updates on resources.
    - `DELETE` for removing resources.
- Ensure the effect of these requests aligns with the resource's nature (collection or individual).

### Operations with Payloads

#### Use `POST` for Creating a New Resource

Example:
```http
POST /api/customers
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john.doe@example.com"
}
```
In this example, a POST request creates a new customer resource with the provided JSON payload.

#### Use `PUT` for Updating a Resource
Example:
```http
PUT /api/products/123
Content-Type: application/json

{
    "name": "Updated Product Name",
    "price": 19.99
}
```
Here, a PUT request updates the product with ID 123 using the provided JSON payload.

#### Use `PATCH` for Partially Updating a Resource
Example:

```http
PATCH /api/orders/456
Content-Type: application/json-patch+json

[
     { "op": "replace", "path": "/status", "value": "shipped" }
]
```
This PATCH request partially updates the status of order 456 using a JSON patch document.

### Triggering Operations
#### User `POST` for Triggering an Operation
Example:
```http
POST POST /api/orders/dispatch
Content-Type: application/json

{
"orderIds": [123, 124, 125]
}
```
To avoid confusion and ensure clarity in your API design, you should aim for a clear distinction between resource creation and action triggering. 
For example, if you want to trigger an action to ship specific orders, you might consider using a more explicit naming convention for your action.

## 3. Conform to HTTP Semantics

- Utilize appropriate HTTP status codes (e.g., `200`, `201`, `204`, `404`, `415`, `400`, `409`) for different scenarios.
- Specify correct `Content-Type` and `Accept` headers for media types (e.g., JSON).
- Implement asynchronous operations for long-running tasks, with status endpoints.
- Use HTTP `PATCH` for partial updates with JSON merge or JSON patch formats.
- Utilize HTTP `DELETE` for resource removal.

## 4. Use Media Types

- Exchange representations of resources using media types (e.g., `application/json`).
- Ensure your API supports common non-binary data formats, such as JSON.
- Set the `Content-Type` header in requests to specify the format of the representation.

## 5. Filter and Paginate Data

- Allow clients to filter results using query strings (e.g., `/orders?minCost=n`).
- Implement pagination with query parameters like `limit` and `cursor` (e.g., /orders?limit=25&cursor=aQwdseD).
- Set meaningful defaults for optional query parameters.
- Provide metadata indicating the total number of resources available in the collection.
- Optionally support sorting with a `sort` parameter (e.g., `/orders?sort=ProductID`).

## 6. API Versioning

- Implement versioning to handle changes to your API over time.
- Include version information in the URI (e.g., /v1/customers) or as a header (e.g., `Accept: application/vnd.myapi.v1+json`).
- Document and communicate versioning changes clearly to API consumers.
- Maintain backward compatibility for existing API versions whenever possible.

## 7. Security

- Implement authentication and authorization mechanisms to secure your API.
- Use industry-standard security protocols such as OAuth 2.0 or API keys.
- Enforce secure communication using HTTPS.
- Validate and sanitize input data to prevent security vulnerabilities (e.g., SQL injection, XSS).
- Implement rate limiting and access controls to protect against abuse.
- Regularly audit and monitor API security.

## 8. Consistency

- Maintain a consistent naming convention for endpoints, methods, and data structures.
- Standardize error responses and status codes across the API.
- Use consistent versioning practices.
- Ensure consistency in data representations (e.g., JSON) and response formats.
- Document API usage and conventions comprehensively for developers.
- Establish and enforce coding and design standards for API development.

## 9. Consider Asynchronous Operations

- Make operations asynchronous if they may take a while to complete.
- Return HTTP status code 202 (Accepted) for accepted asynchronous requests.
- Provide status endpoints for clients to monitor the progress of asynchronous operations.

## 10. Use Idempotent PUT Requests

- Ensure that `PUT` requests are idempotent, meaning they produce the same result when applied multiple times.
- Avoid introducing side effects in `PUT` requests that could change the resource state differently with each request.

By following these best practices, you'll create a REST API that is not only efficient but also intuitive and developer-friendly.
It will improve the overall experience for both API consumers and maintainers.

## Advanced Considerations

### Avoid Chatty APIs

- Minimize the number of requests required for a client to obtain necessary data.
- Denormalize data when necessary to combine related information into larger resources.
- Balance denormalisation against the overhead of fetching unnecessary data.

### Decouple API from Data Sources

- Avoid exposing every database table as a collection of resources.
- Think of your API as an abstraction of the database.
- Introduce a mapping layer between the database and the API if needed.

### Keep Resource URIs Simple

- Avoid overly complex resource URIs with multiple levels of relationships.
- Ensure that references to related resources can be easily navigated from a resource's URI.
- Use resource URIs in a hierarchy (e.g., /customers/1/orders) to improve intuitiveness.

### Logging and Observability

#### Logging Best Practices

- Implement comprehensive logging throughout your API to capture important events, errors, and debugging information.
- Ensure log levels (e.g., INFO, ERROR) provide the appropriate level of detail.
- Use structured log formats (e.g., JSON) for easier parsing and analysis.
- Log relevant information, including request and response details, timestamps, and contextual data.
- Centralize and manage logs using dedicated logging solutions or tools.
- Consider log rotation and retention policies to manage log file sizes.

#### Observability Best Practices

- Implement observability mechanisms to gain insights into your API's behavior and performance.
- Use distributed tracing tools to trace requests across services and endpoints.
- Collect and analyze metrics on API usage, response times, error rates, and resource utilization.
- Implement health checks to monitor the status of your API and its dependencies.
- Set up alerts and notifications to proactively detect and respond to issues.