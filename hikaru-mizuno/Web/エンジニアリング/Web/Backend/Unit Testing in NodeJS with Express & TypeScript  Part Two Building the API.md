---
URL: https://dev.to/abeinevincent/unit-testing-in-nodejs-with-express-typescript-jest-part-three-writing-unit-tests-a-comprehensive-guide-3ipj
Glasp URL: https://glasp.co/4lnt6ccBTsO94gGCU64ypnvR8Af1/p/3efa1cdfeceefc8355e4
Tags: []
Last updated: 2025-02-24
---
#### Highlights & Notes

> Folder Structure for Unit Tests

> ![](https://media2.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzxpzfr8i5qf5aj2z32g7.png)

> Replace code inside app.test.ts with the following code

> beforeAll(async () => {  // Set up: Establish the MongoDB connection before running tests

> afterAll(async () => {  // Teardown: Close the MongoDB connection after all tests have completed

> // Unit test for testing initial route ("/")describe("GET /", () => {

> establishing a MongoDB connection before tests begin and closing it afterward. The primary test case verifies the behavior of the root route ("/"), ensuring it responds with a welcome message and a 200 status.

> Testing models allows us to validate the structure, functionality, and interactions with the database.

> The beforeAll hook orchestrates the setup process before running any tests, ensuring a valid MongoDB connection is established based on the MONGODB_URL environment variable. It throws an error if the variable is not defined, alerting the developer to set the MongoDB connection string. On the other hand, the afterAll hook handles cleanup after all tests are completed, closing the MongoDB connection to prevent resource leaks and maintain a clean testing environment.

> Unit Tests for Database Models

> Before running the tests, it establishes a connection to a MongoDB database specified in the environment variable MONGODB_URL. The test suite covers creating a new user, ensuring the uniqueness of email and username, retrieving all users, updating an existing user, fetching a user by ID, and finally, deleting an existing user. The tests use the User model from the src/models/User file and leverage various assertions to verify the expected behavior of these operations.

> Writing UnitTests for Services

> Writing Unit Tests for Controllers

> Delving into token verification, the suite rigorously examines scenarios of both valid and invalid tokens, intricately assessing the resilience of our verification mechanism. In cases where a valid token is presented, the verification process is expected to seamlessly proceed, ensuring that the user details are defined. Conversely, when confronted with an invalid token, our suite orchestrates a precise response, safeguarding our application against unauthorized access. As the final act of diligence, the suite gracefully resets environment variables, leaving the testing landscape pristine.

> Leveraging the power of mocking, we intentionally isolate the service from external dependencies, ensuring a controlled testing environment. Our suite encompasses crucial aspects, such as creating a new user, logging in a user, retrieving all users, and deleting an existing user.

> In the test suite for the User Controller, we employ thorough mocking using jest.mock to isolate the UserService, ensuring controlled testing of the controller's functionalities.

> Notably, the jest.mock line is crucial, replacing the actual UserService with a mock implementation to create a controlled environment for testing.

> Writing Unit Tests for Routes


