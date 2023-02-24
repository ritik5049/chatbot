## Chat-bot

This is a Vue.js application for a chatbot that can search for articles based on user queries and extract text from URLs. It uses an API built with Node.js and Express to handle the backend functionality.

Getting started

To run the application, you need to have Node.js and npm installed on your machine. You can download them from the official website: https://nodejs.org/

- Clone the repository to your local machine:
```
git clone https://github.com/your-username/chat-bot.git
```

- Install the dependencies by running the following command in the project directory:
```
npm install
```

- Start the application by running the following command:
```
npm run serve
```

This will start the development server and open the application in your default web browser at http://localhost:8080/

### Usage

The chatbot interface allows users to type in queries and receive responses from the bot. The bot can perform two types of actions:

- Extract text from a URL: If the user types in a valid URL, the bot will try to extract the main text from the webpage and provide a downloadble link for the text file.

- Search for articles based on a query: If the user types in any other text, the bot will search for articles containing that text and display their titles and snippets as responses.

### API

The backend API provides the following endpoints:

- GET /articles: Returns a list of articles that match a given query parameter.

- POST /articles: Takes a URL parameter and returns the title and main text of the webpage as a JSON object.
Technologies used

### Technologies used

- Vue.js
- Node.js
- Axios
