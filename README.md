# -> Spring AI Demo — Google Gemini

A **Spring Boot** application demonstrating **Spring AI integration with Google Gemini**.

---

## -> High-Level Design

### => Components & Responsibilities

| Component        | Role                                                                                     |
| ---------------- | ---------------------------------------------------------------------------------------- |
| `HomeController` | Serves the Thymeleaf web UI at `GET /`                                                   |
| `AIController`   | REST endpoint — accepts a prompt query param, returns AI-generated text                  |
| `AIService`      | Wraps Spring AI `ChatClient`; sends the prompt to Google Gemini and returns the response |
| `Gemini API`     | External LLM (`gemini-2.5-flash`) hosted on Google Cloud; performs text generation       |

---

## -> UI Overview

The application serves a clean single-page chat interface at:

=> http://localhost:8080

---

### -> Initial State

```
Spring AI Demo  [Gemini]

Ask anything

┌──────────────────────────────────────────────┐
│ e.g. Explain Spring AI in simple terms...    │
└──────────────────────────────────────────────┘

[ Ask Gemini ]
```

---

### -> While Waiting for Response

```
[ Ask Gemini ] (disabled)

↻ Thinking...
```

---

### -> Response State

```
Spring AI is a framework within the Spring ecosystem
that simplifies building AI-powered applications...
```

---

=> **Tip:**
Press `Ctrl + Enter` (or `Cmd + Enter` on macOS) to submit without clicking the button.

---

## -> Step 1: Create a Gemini API Key

1. Go to **Google AI Studio**
2. Sign in with your Google account
3. Click **"Create API key"**
4. Copy the generated key

---

## -> Step 2: Configure the API Key

Open:

```
src/main/resources/application.properties
```

Add:

```properties
spring.application.name=spring-ai-demo
spring.ai.google.genai.api-key=YOUR_GEMINI_API_KEY
spring.ai.google.genai.chat.options.model=gemini-2.5-flash
```

### -> Secure Way (Recommended)

```properties
spring.ai.google.genai.api-key=${GEMINI_API_KEY}
```

Set environment variable:

```bash
export GEMINI_API_KEY=your_key_here
```

---

## -> Step 3: Run the Application

```bash
mvn clean install
mvn spring-boot:run
```

App will start at:

=> http://localhost:8080

---

## -> Step 4: Test the API

### Using CURL

```bash
curl "http://localhost:8080/api/ai/ask?prompt=What+is+Spring+AI?"
```

### Using Browser

```
http://localhost:8080/api/ai/ask?prompt=Hello
```

---

## * Tech Stack

* Java
* Spring Boot
* Spring AI
* Google Gemini API
* Thymeleaf

----

## -> License

This project is for learning/demo purposes.



***** Application Flow **********

###  Client Layer (Browser)

* User opens the application in the browser
* UI: `index.html` (Thymeleaf)

---

###  Step-by-Step Flow

1. **User Request (Home Page)**

   * Browser → `GET /`
   * Handled by: `HomeController`

3. **Load UI**

   * `HomeController` returns Thymeleaf template
   * Browser displays `index.html`

4. **User Sends Prompt**

   * User enters a question and clicks **Ask Gemini**
   * Browser → `GET /api/ai/ask?prompt=...`

5. **REST Controller**

   * Request handled by: `AIController`
   * Calls service method: `generateText(prompt)`

6. **Service Layer**

   * `AIService` uses Spring AI `ChatClient`
   * Sends request to Gemini API

7. **External API Call**

   * `AIService` → HTTP call → Google Gemini (`gemini-2.5-flash`)

8. **AI Processing**

   * Gemini API processes the prompt
   * Generates response

9. **Response Flow Back**

   * Gemini → `AIService` (AI response)
   * `AIService` → `AIController` (String response)
   * `AIController` → Browser (JSON/text response)

10. **UI Update**

   * Browser displays AI-generated response

---

## => Summary Flow

```
Browser (UI)
   ↓
HomeController
   ↓
Thymeleaf UI Loaded
   ↓
User Prompt → AIController
   ↓
AIService (ChatClient)
   ↓
Google Gemini API
   ↓
Response back → UI
```
