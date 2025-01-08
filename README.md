# langchain-tutorial

## 1. **What Are Chat Models?**

Chat models are **smart conversation tools**. Instead of just reading plain text, they understand **messages** and their roles, like:
- **User Messages**: What you say or ask.
- **AI Messages**: The assistant's replies.
- **System Messages**: Rules or instructions for the assistant.

They’re great for creating chats or apps that need clear, role-based communication.

### **Why Use Chat Models?**
1. **Designed for Conversations**: Understand who’s saying what (AI, User, or System).
2. **Works with Simple Text**: Even plain text can be used—they’ll convert it for you.
3. **Specialized Models**: Some are trained for tasks like tool usage or data lookups.


### **How to Set Up Chat Models**

| **Setting**        | **What It Does**                               | **Example**                               |
|--------------------|-----------------------------------------------|-------------------------------------------|
| **model**          | Choose the AI (e.g., GPT-4, Claude).          | *"gpt-4"*                                 |
| **temperature**    | Controls creativity.                          | *0.2 = focused, 1.0 = creative.*          |
| **timeout**        | How long to wait for a response.              | *Set to 60 seconds.*                      |
| **max_tokens**     | Limits the length of replies.                 | *200 tokens for short answers.*           |
| **stop**           | Stops output at specific words.               | *Stop at: "Goodbye."*                     |
| **api_key**        | Your access key for the model.                | *Key from OpenAI or Anthropic.*           |



### **Simple Example**
1. **User Says**: *"What’s the weather?"*
2. **System Rule**: *"You are a weather assistant."*
3. **AI Replies**: *"It’s sunny with a high of 25°C."*

---

## 2. Messages
Here’s an even simpler and clearer version of the table:

| **Message Type**       | **Who Sends It?**   | **What It Does**                                                                 | **When to Use It**                             | **Simple Example**                                |
|-------------------------|---------------------|----------------------------------------------------------------------------------|-----------------------------------------------|--------------------------------------------------|
| **HumanMessage**        | User               | The user’s input or question.                                                   | When the user sends a message or asks a question. | *"What’s the weather like today?"*               |
| **AIMessage**           | Assistant          | The assistant’s reply or response.                                               | When the assistant answers the user.          | *"It’s sunny and 25°C outside."*                |
| **SystemMessage**       | System             | Instructions or rules for how the assistant should behave.                       | To guide the assistant’s behavior.            | *"You are a friendly assistant who loves jokes."* |
| **ToolMessage**         | Tool               | Results or data returned from an external tool or function.                      | When tools are used to fetch or calculate data. | *"The current stock price for AAPL is $150."*    |
| **FunctionMessage**     | Function (Old)     | Results from functions in older systems (not used anymore, replaced by ToolMessage). | Avoid using; use ToolMessage instead.          | *"The weather is sunny and 25°C."*              |

### Simple Breakdown:
1. **HumanMessage**: User speaks to the assistant.
2. **AIMessage**: Assistant speaks back to the user.
3. **SystemMessage**: Sets rules for the assistant (like "Be polite" or "Speak only in English").
4. **ToolMessage**: Assistant uses a tool (like a calculator or database) and shows the result.
5. **FunctionMessage**: Old method for showing tool results; avoid using this.

This table uses simple terms and focuses on “who, what, when” with basic examples to make it beginner-friendly.
