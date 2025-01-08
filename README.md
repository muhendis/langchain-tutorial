
# A. General

## 0. **LangChain Architecture Overview**


 **Core Packages**

| **Package**          | **What It Does**                                                                                      | **Key Features**                                                                                   |
|----------------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **langchain-core**   | The foundation of LangChain. Defines the basic building blocks for working with AI tools.             | - Lightweight dependencies.<br>- Base interfaces for components like LLMs, vector stores, retrievers. |
| **langchain**        | The main package for creating AI-powered workflows (chains, agents, etc.).                           | - Generic cognitive workflows.<br>- Works across all integrations (not tied to any specific one). |



**Integration Packages**

| **Package**              | **What It Does**                                                                                      | **Key Features**                                                                                   |
|--------------------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **langchain-community**  | Contains third-party integrations contributed by the community.                                       | - Supports various LLMs, vector stores, and retrievers.<br>- Dependencies are optional for flexibility. |
| **Partner Packages**     | Popular third-party integrations are split into their own packages for better support.               | - Examples: *langchain-openai, langchain-anthropic.*<br>- Better focus and updates for key tools.   |


### **Specialized Extensions**

| **Package**      | **What It Does**                                                                                      | **Key Features**                                                                                   |
|------------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **langgraph**    | Helps build advanced, stateful workflows using a graph structure (nodes = steps, edges = connections). | - For multi-step/multi-actor processes.<br>- High-level and low-level APIs for custom flows.       |
| **langserve**    | Makes it easy to deploy LangChain workflows as REST APIs.                                             | - Get production-ready APIs quickly.<br>- Simple deployment for AI applications.                  |
| **LangSmith**    | A developer platform for debugging, testing, and monitoring LangChain applications.                   | - Debug and evaluate applications.<br>- Track and monitor performance over time.                  |



**How It All Fits Together**

1. **Start with Core**: Use **langchain-core** and **langchain** to create AI workflows.
2. **Add Integrations**: Bring in **langchain-community** or partner packages (like OpenAI) for specific tools.
3. **Advanced Use**:
   - Use **langgraph** for complex workflows.
   - Deploy your app with **langserve**.
   - Debug and improve with **LangSmith**.


 **Example Use Case**
1. **Core Workflow**: Use **langchain** to define a chatbot.
2. **Integration**: Add **langchain-openai** for GPT integration.
3. **Deployment**: Use **langserve** to turn it into a REST API.
4. **Debugging**: Monitor the API’s performance using **LangSmith**.

---
## 1. **What is LangChain Expression Language (LCEL)?**


LCEL is a **declarative way** to connect different LangChain components into powerful workflows. It allows you to build everything from simple AI workflows (e.g., "prompt + LLM") to highly complex ones with hundreds of steps—all ready for production with no code changes.


 **Why Use LCEL?**

| **Feature**                 | **What It Does**                                                                 | **Why It’s Useful**                                                                                 |
|-----------------------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **Streaming Support**        | Streams tokens from the LLM to output parsers in real-time.                     | Get results incrementally, as fast as the LLM can generate them.                                   |
| **Async Support**            | Works both synchronously (for prototypes) and asynchronously (for production).  | Handle many requests efficiently and use the same code for prototyping and deployment.             |
| **Optimized Parallel Steps** | Executes steps in parallel when possible (e.g., fetching from multiple sources).| Reduces latency for faster results.                                                               |
| **Retries and Fallbacks**    | Adds retry mechanisms and alternative steps to handle failures.                 | Makes workflows more reliable at scale.                                                           |
| **Access Intermediate Results** | Lets you view intermediate outputs during execution.                        | Useful for debugging or showing progress to users.                                                |
| **Input/Output Schemas**     | Automatically generates schemas (Pydantic/JSONSchema) for inputs and outputs.   | Ensures data validation and compatibility, especially for APIs.                                    |
| **Seamless LangSmith Logging** | Tracks all steps automatically for debugging and observability.               | Makes complex workflows easier to monitor and debug.                                               |


 **How LCEL Works**

1. **Chain Components Together**: LCEL connects components like LLMs, retrievers, and parsers into workflows.
2. **Run Consistently**: Use the same chains in prototypes and production without changing code.
3. **Customize and Control**: Customize prompts and outputs, avoiding hidden details like in legacy chains.


 **Key Features for Customization**

| **Feature**         | **What It Does**                                                                   | **Example**                                                                                     |
|---------------------|------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| **Runnable Protocol** | A standard interface for creating custom workflows.                              | Components like LLMs, retrievers, and parsers follow this protocol.                           |
| **Stream**          | Stream back chunks of the response in real time.                                  | Get partial results before the full response is ready.                                         |
| **Invoke**          | Call the chain with a single input.                                               | Pass one question to the workflow.                                                            |
| **Batch**           | Call the chain with a list of inputs.                                             | Process multiple questions at once.                                                           |
| **Async Methods**   | Supports async methods like `astream`, `ainvoke`, and `abatch`.                   | Use asyncio to handle concurrency for faster performance.                                      |
| **Intermediate Logs** | Logs intermediate steps with methods like `astream_log` and `astream_events`.    | View results of each step as they happen, ideal for debugging complex workflows.              |



 **Input and Output Types by Component**

| **Component**      | **Input Type**                                | **Output Type**                            |
|--------------------|-----------------------------------------------|-------------------------------------------|
| **Prompt**         | Dictionary                                   | PromptValue                               |
| **ChatModel**      | Single string, chat messages, or PromptValue | ChatMessage                               |
| **LLM**            | Single string, chat messages, or PromptValue | String                                    |
| **OutputParser**   | The output of an LLM or ChatModel            | Depends on the parser                     |
| **Retriever**      | Single string                                | List of Documents                         |
| **Tool**           | String or dictionary (depends on the tool)   | Depends on the tool                       |



 **Why LCEL is Better than Legacy Chains**
- **Customizable**: Prompts and workflows are no longer hidden.
- **Flexible**: Works across different models and components.
- **Production-Ready**: From prototyping to deployment without changing code.



 **Example Workflow with LCEL**

1. **Input**: User sends a query.
2. **Intermediate Steps**: 
   - Retrieve documents using multiple sources in parallel.
   - Pass the retrieved documents to an LLM for summarization.
   - Parse the LLM's output using a custom parser.
3. **Final Output**: Return a structured answer to the user, streamed in real-time.


---

# B. Components

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
