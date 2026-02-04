# Instructor Guide: Connecting to AI and ML APIs

## Module Overview

In this module, learners connect web applications to external AI models and APIs, focusing on secure and efficient data transfer. The session bridges the gap between static frontend code and dynamic AI intelligence by implementing a robust backend middleware layer.

## Learning Objectives

By the end of this session, students will be able to:

* **Integrate** AI APIs to send, receive, and render model outputs in a web application.
* **Construct** a secure middleware layer to abstract API credentials and manage data flow between the client and the AI provider.
* **Analyze** the structure of JSON payloads to effectively parse nested API responses.

## Session Pacing Guide (120 Minutes total)

| Duration | Segment | Instructor Action |
| --- | --- | --- |
| **20 min** | **The Role of APIs & Security** | Discuss Direct vs. Indirect access. Use the "Waiter" analogy to explain Middleware. Explain why frontend calls are insecure. |
| **25 min** | **Anatomy of an API Request** | Live coding: Build a JSON payload. Walk through `model`, `messages`, and `temperature`. |
| **25 min** | **Building the Middleware** | Live demo: Create a simple Express route. Show how to swap the hardcoded prompt for a dynamic variable from `req.body`. |
| **20 min** | **Parsing & Rendering** | Show the raw JSON response from OpenAI. Demonstrate how to "drill down" to the actual text content (`choices[0].message.content`). |
| **30 min** | **Lab: Creating an AI Middleware Layer** | Facilitate the lab. Help students troubleshoot `async/await` errors and Postman configurations. |

---

## Delivery Notes & Teaching Tips

### 1. The Middleware Pattern (The "Waiter" Analogy)

* **Concept:** Students often struggle with *why* they need a server if they can just call the API from React.
* **Analogy:**
* **Client (Customer):** Orders food.
* **Middleware (Waiter):** Takes the order to the kitchen. The waiter knows the secret code to enter the kitchen; the customer does not.
* **AI API (Kitchen):** Cooks the food (Processes data).


* **Visual Aid:** Draw the flow: `Client -> [Server + Key] -> OpenAI`. Cross out a line connecting `Client -> OpenAI` directly to emphasize security.

### 2. Anatomy of a Request

* **Parameter Tuning:** Explain `temperature` carefully.
* *Low Temperature (0.2):* "The accountant." Predictable, boring, accurate.
* *High Temperature (0.8+):* "The poet." Creative, hallucination-prone.


* **Interactive Demo:** Use the OpenAI Playground or Postman to show the same prompt generating different results by changing the temperature.

### 3. Parsing JSON Responses

* **"Data Drilling":** Beginners often get overwhelmed by the large JSON object returned by AI.
* **Activity:** Put a complex JSON response on the screen. Ask a student to write the dot-notation path on the whiteboard to reach specific data (e.g., `response.usage.total_tokens`).

---

## Lab Guide: Creating an AI Middleware Layer

**Lab Objective:** Build a middleware layer that connects to an AI API and displays model results.

### Setup

* **Environment:** Node.js/Express project initialized in the previous module.
* **Tools:** Postman for testing routes before connecting the frontend.

### Common Student Issues & Solutions

* **Issue:** `Cannot read property 'content' of undefined`.
* *Solution:* The student likely didn't wait for the promise to resolve. Check for `await` keywords on the `fetch` call and the `.json()` conversion.


* **Issue:** **CORS Error** when connecting the frontend.
* *Solution:* They are calling the server from a different port (e.g., React on 3000, Node on 5000). Ensure the `cors` package is installed and used (`app.use(cors())`).


* **Issue:** The AI replies with valid JSON, but the frontend shows `[object Object]`.
* *Solution:* They are trying to render an object directly in React. They need to render the specific string property (e.g., `{result.content}`).



### Debrief Questions

At the end of the lab, ask:

1. "Why did we pass the user's message into the `messages` array instead of just sending a plain string?" (Context/Chat History).
2. "If you wanted to limit users to only 5 requests per day, where would you write that logic?" (In the Middleware).

---

## Resources for the Instructor

* **Reference:** [Express.js Basic Routing](https://expressjs.com/en/starter/basic-routing.html)
* **Reference:** [CORS Middleware Documentation](https://www.npmjs.com/package/cors)
* **AI Prompt Bank:**
* *"I'm getting a 429 'Too Many Requests' error. What does this mean?"* (Rate limiting/Quota exceeded).
* *"Write a JavaScript function to safely extract the content from an OpenAI response, handling potential errors."*