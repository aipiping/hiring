## Development Task for AI Piping with Distributed Processing

Backend, Python + FastAPI 

**Objective:** Create a scalable FastAPI application that serves an endpoint to recommend three things to do in a given country during a specific season by consulting the OpenAI API. Additionally, integrate a distributed component for background processing to handle the OpenAI API calls asynchronously, store the results in MongoDB, and signal completion. Use Docker Compose to manage the application components.

### Instructions:

1. **Setup**:
   - Initialize a new Python project.
   - Install FastAPI, MongoDB driver, and any other required libraries.

2. **API Endpoint**:
   - Design an endpoint with a custom route for travel recommendations.
   - This endpoint should accept two query parameters:
     - `country`: The country for which the recommendations are to be fetched.
     - `season`: The season in which the recommendations are desired (e.g., "summer", "winter").
     - Both parameters are required. The season must validate that one of the four seasons is chosen.
   - Use localhost:3000.
   - No authentication required for the endpoint/API.
   - No encryption, no HTTPS... Keep it simple.

3. **Request Handling**:
   - When a request is made to the endpoint, generate a unique identifier (UID) for the request.
   - Return the UID to the user immediately.
   - Offload the processing of the request to a background component.

4. **Background Processing with Distributed Component**:
   - Integrate one of the following distributed components for background processing:
     - **Kafka**: Use Kafka to send the request as a message to a Kafka topic and consume it with a separate worker.
     - **Prefect**: Use Prefect for orchestrating the background tasks.
   - The background component should:
     - Receive the request.
     - Make the OpenAI API call.
     - Process the response.
     - Store the recommendations in MongoDB.
     - Signal completion of the task.
   - Ensure that the FastAPI endpoint can query the status of the background task and retrieve the recommendations once the task is complete.

5. **Data Storage**:
   - Store the recommendations in MongoDB with the UID as the key.
   - Structure the data to include the country, season, recommendations, and a status field (e.g., pending, completed).

6. **Completion Signaling**:
   - Once the background task is complete, update the status in MongoDB to indicate that the recommendations are available.
   - Implement a mechanism (e.g., Kafka consumer or Prefect future) to notify the API that the task is complete and data can be queried.

7. **Data Retrieval Endpoint**:
   - Design an endpoint to retrieve the recommendations using the UID.
   - This endpoint should:
     - Accept the UID as a query parameter.
     - Check the status in MongoDB.
     - If the status is "completed", return the recommendations.
     - If the status is "pending", inform the user that the data is not yet available.
     - If the UID is not found, return an appropriate error message.

8. **Containerization**:
   - Use Docker to containerize the FastAPI application, the chosen background processing component, and MongoDB.
   - Create a `docker-compose.yml` file to manage the different services and ensure they work together seamlessly.

9. **Response Example**:
   - When initiating the request:
     ```json
     {
       "uid": "1234567890abcdef"
     }
     ```
   - When retrieving the recommendations and the task is completed:
     ```json
     {
       "uid": "1234567890abcdef",
       "country": "Canada",
       "season": "winter",
       "recommendations": [
         "Go skiing in Whistler.",
         "Experience the Northern Lights in Yukon.",
         "Visit the Quebec Winter Carnival."
       ],
       "status": "completed"
     }
     ```
   - When retrieving the recommendations and the task is still pending:
     ```json
     {
       "uid": "1234567890abcdef",
       "status": "pending",
       "message": "The recommendations are not yet available. Please try again later."
     }
     ```
   - When the UID is not found (make sure to return the appropriate HTTP status code, 404):
     ```json
     {
       "error": "UID not found",
       "message": "The provided UID does not exist. Please check the UID and try again."
     }
     ```

10. **Additional (Optional, not required)**:
    - Use Beanie or another MongoDB ODM for Python to interact with MongoDB.
    - Implement error handling and logging throughout the application.
    - Write tests for the application to ensure it behaves as expected.
    - If you consider yourself full stack, and want to showcase your skills, you can create a simple frontend that calls this API.

11. **Submission**:
    - Push your code to a public GitHub repository.
    - Include a `README.md` with clear instructions on how to run the application, test it, and any other relevant information.

### Evaluation Criteria:

1. **Functionality**: Does the application work as described?
2. **Code Quality**: Is the code organized, clean, and free of bugs?
3. **API Design**: Is the API intuitive and easy to understand?
4. **Integration**: Is the OpenAI GPT API integrated seamlessly, and are errors handled gracefully?
5. **Scalability**: How well is the background processing component integrated and how scalable is the solution?
6. **Containerization**: Is the Docker setup clear, and does it work without issues?
7. **Documentation**: Are the setup and usage instructions clear?

### Documentation:

- FastAPI documentation: [https://fastapi.tiangolo.com/](https://fastapi.tiangolo.com/)
- OpenAI GPT API documentation: [https://platform.openai.com/docs/api-reference](https://platform.openai.com/docs/api-reference)
- Kafka documentation: [https://kafka.apache.org/documentation/](https://kafka.apache.org/documentation/)
- Prefect documentation: [https://docs.prefect.io/](https://docs.prefect.io/)
- Docker Compose documentation: [https://docs.docker.com/compose/](https://docs.docker.com/compose/)
- MongoDB documentation: [https://docs.mongodb.com/](https://docs.mongodb.com/)
