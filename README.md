REST API Documentation for Authentication, File Upload, and Object Sync

This documentation outlines the endpoints and usage of the REST API for user authentication, file uploads with cryptic URLs, and object synchronization.

1. Authentication Endpoint

POST /auth

Authenticate the user with an email and password. If successful, the session is created, and a user object is returned.

	•	URL: /auth
	•	Method: POST
	•	Request Body (JSON):

{
  "email": "user@example.com",
  "password": "userpassword"
}


	•	Response (Success - 200 OK):

{
  "name": "User Name",
  "email": "user@example.com",
  "role": "user"
}


	•	Response (Error - 401 Unauthorized):

{
  "error": "Invalid email or password"
}


	•	Response (Error - 400 Bad Request):

{
  "error": "Email and password are required"
}



DELETE /auth

Logs out the current user and destroys the session.

	•	URL: /auth
	•	Method: DELETE
	•	Response (Success - 200 OK):

{
  "message": "Logged out successfully"
}



2. File Upload Endpoint

POST /files

Allows the user to upload a file and returns a cryptic URL for the uploaded file. This endpoint only accepts multipart form data for file uploads.

	•	URL: /files
	•	Method: POST
	•	Request Body: Multipart form data (use a file key for uploading the file)
	•	Response (Success - 200 OK):

{
  "message": "File uploaded successfully",
  "fileUrl": "/uploads/5b1c4c1d6c3a4b9f16b10f1f7098e7f1.jpg"
}


	•	Response (Error - 400 Bad Request):

{
  "error": "No file uploaded"
}


	•	Response (Error - 401 Unauthorized):

{
  "error": "Unauthorized"
}



3. Object Sync Endpoint

POST /objects

Sends new objects from the client, stores them on the server, and returns objects that have been added or changed since the provided timestamp.

	•	URL: /objects
	•	Method: POST
	•	Request Body (JSON):

{
  "timestamp": 12391273908127,
  "newObjects": [
    {
      "content": {},
      "owners": ["user@example.com"],
      "type": "block",
      "time": 12391273908129
    }
  ]
}


	•	Response (Success - 200 OK):

[
  {
    "content": {},
    "owners": ["user@example.com"],
    "type": "block",
    "time": 12391273908129
  }
]


	•	Response (Error - 400 Bad Request):

{
  "error": "Timestamp and newObjects are required"
}


	•	Response (Error - 401 Unauthorized):

{
  "error": "Unauthorized"
}



Summary of Responses

Common Response Codes:

	•	200 OK: The request was successful.
	•	400 Bad Request: The request was malformed or missing required data.
	•	401 Unauthorized: The user is not authenticated (login required).
	•	500 Internal Server Error: There was an error on the server.

Authentication Flow:

	1.	Login:
	•	Send a POST /auth request with the user’s email and password.
	•	On success, the server returns the user object, and a session is created.
	2.	Logout:
	•	To logout, send a DELETE /auth request, which ends the session.

File Upload Flow:

	1.	Upload File:
	•	Use POST /files with a multipart form upload.
	•	The server returns a cryptic URL for accessing the uploaded file.

Object Sync Flow:

	1.	Send Objects:
	•	Clients send a POST /objects request with new objects and the timestamp of the last synchronization.
	•	The server stores the new objects and returns any objects that were added or modified after the provided timestamp.

Example Usage Flow:

	1.	Login (POST /auth):
	•	Client authenticates and receives user object.
	2.	Upload a File (POST /files):
	•	Client uploads a file and receives a cryptic URL for it.
	3.	Sync Objects (POST /objects):
	•	Client sends new objects and timestamp, then receives any new or modified objects from the server.

This API provides basic user authentication, file uploads, and object syncing, making it suitable for a variety of applications requiring secure user interaction and data synchronization.
