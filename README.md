
# A. General

## 1. **LangChain Architecture Overview**


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
## 2. **What is LangChain Expression Language (LCEL)?**


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

---

## 3.  **What are LLMs?**

LLMs (Language Models) are AI tools that take **text as input** and give **text as output**. They’re great for tasks like writing, summarizing, or answering questions.



 **Key Features**

| **Feature**             | **What It Does**                                                   | **Why It’s Useful**                          |
|-------------------------|-------------------------------------------------------------------|---------------------------------------------|
| **Text Input/Output**   | You give it text, and it gives you text back.                     | Great for simple tasks like generating ideas or summaries. |
| **Works with Messages** | In LangChain, LLMs can handle chat-style messages, too.           | Makes them flexible for different workflows. |
| **Third-Party Models**  | LangChain connects to AI providers like OpenAI.                  | Lets you use powerful, well-known models.    |



 **LLMs vs. Chat Models**

| **LLMs**                          | **Chat Models**                          |
|-----------------------------------|------------------------------------------|
| Work with plain text.             | Work with chat-style messages.           |
| Better for simple tasks.          | Better for conversations or workflows.   |
| Often older technology.           | Newer and more advanced technology.      |



 **Why Use LLMs?**
- For simple tasks like:
  - Writing text.
  - Summarizing content.
  - Answering questions.
- When you don’t need advanced conversation handling.



 **How LangChain Improves LLMs**
- **Chat Compatibility**: LLMs can handle messages as if they’re chat models.
- **Third-Party Integration**: Connects to top providers like OpenAI.
- **Unified Tools**: Makes LLMs and Chat Models work in the same way.



 **Important Note**
Most newer AI tools are **Chat Models**, which are better for modern tasks. If you’re unsure, Chat Models might be a better fit.


 **How to Start**
1. Choose a model from providers like OpenAI.
2. Use LangChain to connect and enhance it.
3. Focus on simple, text-based tasks.

---


### 4. **What are Prompt Templates?**

Prompt Templates are tools that help you create clear instructions for AI models. They take your inputs, organize them, and turn them into something the AI can understand.



### **How Prompt Templates Work**

1. **Input**: You provide variables (like `{"topic": "cats"}`).
2. **Process**: The template fills in the blanks with your variables.
3. **Output**: It produces a prompt ready for the AI to use.



 **Types of Prompt Templates**

| **Type**               | **What It Does**                                     | **Example**                                   |
|------------------------|-----------------------------------------------------|----------------------------------------------|
| **String Prompt**      | Formats a single sentence.                          | *"Tell me a joke about {topic}"*             |
| **Chat Prompt**        | Formats multiple messages for a conversation.       | System and user roles with dynamic content.  |
| **MessagesPlaceholder**| Lets you insert a list of messages dynamically.     | Add several messages into a conversation.    |



**Examples**

**a. Simple String Prompt**
For single-line prompts.

```python
from langchain_core.prompts import PromptTemplate

template = PromptTemplate.from_template("Tell me a joke about {topic}")
print(template.invoke({"topic": "cats"}))
# Output: "Tell me a joke about cats"
```

 **b. Chat Prompt**
For multi-message conversations.

```python
from langchain_core.prompts import ChatPromptTemplate

template = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("user", "Tell me a joke about {topic}")
])

print(template.invoke({"topic": "cats"}))
# Output: [{"role": "system", "content": "You are a helpful assistant."},
#          {"role": "user", "content": "Tell me a joke about cats"}]
```



**c. MessagesPlaceholder**
To insert a dynamic list of messages.

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.messages import HumanMessage

template = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    MessagesPlaceholder("msgs")
])

print(template.invoke({"msgs": [HumanMessage(content="Hi!")]}))
# Output: [{"role": "system", "content": "You are a helpful assistant."},
#          {"role": "user", "content": "Hi!"}]
```



 **Why Use Prompt Templates?**
- **Simple**: Easily create clear instructions for AI.
- **Flexible**: Handle single-line or multi-message inputs.
- **Reusable**: Use the same template with different inputs.

---



### 5. **What are Output Parsers?**

Output parsers take the **text output** from a language model and transform it into a **structured format** like JSON, CSV, or a Python object. This is helpful when:
- You need **structured data** for downstream tasks.
- You want to **normalize outputs** from language models.



 **Key Features of Output Parsers**

| **Feature**                | **What It Does**                                                                 | **Why It’s Useful**                              |
|----------------------------|----------------------------------------------------------------------------------|------------------------------------------------|
| **Transforms Output**      | Converts raw text from a model into a structured format.                         | Makes it easier to work with model-generated data. |
| **Supports Formats**       | Handles formats like JSON, XML, CSV, and more.                                  | Lets you use data in the format you need.        |
| **Error Correction**       | Some parsers can fix or retry bad outputs using an LLM.                          | Ensures reliable results.                        |



 **Why Use Output Parsers?**
- **Structured Data**: Ideal for tasks like creating JSON objects or tables.
- **Flexibility**: Works with both text and message-based inputs.
- **Error Handling**: Automatically fixes or retries bad outputs with certain parsers.



**Common Output Parsers**

| **Name**            | **Supports Streaming** | **Output Type**            | **When to Use**                                                                 |
|---------------------|------------------------|----------------------------|--------------------------------------------------------------------------------|
| **JSON**            | ✅                     | JSON object                | Best for structured data in JSON format (e.g., APIs or databases).             |
| **XML**             | ✅                     | Dictionary of tags         | When you need XML output (works well with Anthropic models).                   |
| **CSV**             | ✅                     | List of strings            | For creating or handling CSV-like outputs.                                     |
| **OutputFixing**     |                      | Depends on wrapped parser  | Fixes errors by asking an LLM to correct bad outputs.                          |
| **RetryWithError**   |                      | Depends on wrapped parser  | Fixes errors with additional context, like original instructions.              |
| **Pydantic**        |                      | Pydantic model             | For output in a user-defined Python Pydantic model format.                     |
| **YAML**            |                      | Pydantic model             | Similar to Pydantic but encoded in YAML format.                                |
| **PandasDataFrame** | ✅                     | Pandas DataFrame           | For tasks requiring structured data in DataFrame format.                       |
| **Enum**            |                      | Enum                       | Converts responses into one of several predefined categories.                  |
| **Datetime**        |                      | Datetime object            | For outputs in date/time format.                                               |
| **Structured**      |                      | Dictionary of strings      | Simple parser for smaller LLMs, only supports string fields.                   |



**How Output Parsers Work**

1. **Input**: Raw text or message from a language model.
2. **Processing**: The parser transforms the output into the desired format (e.g., JSON, CSV).
3. **Output**: A structured result ready for downstream tasks.


**When to Avoid Output Parsers**
- If your model supports **function/tool calling**, use that instead. Function calling is often more reliable and automatic for structured outputs.


**Examples of Use**

**JSON Output**
Convert model output into JSON.

```python
from langchain_core.output_parsers import JSONOutputParser

parser = JSONOutputParser()
output = parser.parse("Output: {'name': 'John', 'age': 30}")
# Output: {'name': 'John', 'age': 30}
```

**CSV Output**
Create a list of comma-separated values.

```python
from langchain_core.output_parsers import CSVOutputParser

parser = CSVOutputParser()
output = parser.parse("Output: name,age\nJohn,30")
# Output: ['name,age', 'John,30']
```


**Key Takeaways**
- Output parsers are ideal for turning unstructured text into usable formats.
- Choose the parser based on your **desired format** (e.g., JSON, CSV, Pandas).
- Use **function/tool calling** if supported by your model for better reliability.

---

### 6. **What is Chat History?**

Chat History is a way for conversational AI applications to remember and refer to past messages in a conversation. It’s an essential feature that enables the system to maintain context and continuity in interactions.


 **Why is Chat History Important?**
1. **Maintains Context**: Ensures the AI remembers earlier parts of the conversation.
2. **Improves Responses**: Helps generate relevant and coherent replies.
3. **Supports Complex Dialogues**: Allows the AI to handle multi-turn conversations effectively.



 **How Chat History Works in LangChain**

LangChain’s **ChatHistory** is a class that:
1. **Tracks Messages**: It keeps a record of all inputs (user messages) and outputs (AI responses).
2. **Appends to Database**: Stores these messages in a message database.
3. **Loads Past Messages**: Retrieves past messages and includes them in the input for future interactions.



 **Key Features of ChatHistory**
- **Message Tracking**: Automatically records all inputs and outputs from the conversation.
- **Context Persistence**: Loads previous messages as part of the input for the current conversation.
- **Integrates with Chains**: Works with LangChain’s chains to provide context-aware responses.



 **Example Usage**

```python
from langchain_core.memory import ChatHistory

# Initialize ChatHistory
chat_history = ChatHistory()

# Simulate a conversation
chat_history.add_user_message("What's the weather like?")
chat_history.add_ai_message("It's sunny and 25°C.")

# Retrieve messages for future interactions
all_messages = chat_history.messages
print(all_messages)
# Output: [{'role': 'user', 'content': "What's the weather like?"}, 
#          {'role': 'assistant', 'content': "It's sunny and 25°C."}]
```



**Benefits of Chat History**
- **Better User Experience**: Makes conversations feel more natural and engaging.
- **Customizable Memory**: Allows for defining how much history to retain (e.g., last 5 messages).
- **Scalable**: Works with large conversations by using a window of past messages.



**Key Takeaways**
- **Chat History is Essential**: It ensures context and relevance in conversational AI.
- **Integrated with LangChain**: Works seamlessly with chains to enhance conversation quality.
- **Simple and Powerful**: Easy to use but critical for maintaining meaningful interactions.

---

### 7. **What are Documents in LangChain?**

A **Document** in LangChain represents a piece of data, typically text-based, that the system can work with. It organizes this data into a structured object with two key attributes: **content** and **metadata**.


**Key Attributes of a Document**

| **Attribute**        | **What It Is**                                   | **Why It’s Useful**                               |
|----------------------|-------------------------------------------------|-------------------------------------------------|
| **`page_content`**   | The main content of the document as a string.    | Stores the actual text or information to work with. |
| **`metadata`**       | A dictionary of additional information.         | Tracks details like document ID, file name, source, etc. |





**How to Use a Document Example: Creating a Document**

```python
from langchain_core.schema import Document

# Create a Document
doc = Document(
    page_content="LangChain is a framework for building AI applications.",
    metadata={"doc_id": 1, "file_name": "intro.txt"}
)

# Access attributes
print(doc.page_content)
# Output: "LangChain is a framework for building AI applications."

print(doc.metadata)
# Output: {"doc_id": 1, "file_name": "intro.txt"}
```


 **Why Use Documents?**

1. **Organized Data**: Separates content from metadata for better structure.
2. **Flexible Metadata**: Attach any information, like IDs, sources, or timestamps.
3. **Interoperable**: Works seamlessly with LangChain tools like retrievers and memory systems.



 **Key Takeaways**
- A **Document** stores text data and its associated metadata.
- Use **`page_content`** for the main text and **`metadata`** for additional details.
- Documents are the building blocks for handling data in LangChain.

---
### 8. **What are Document Loaders in LangChain?**

Document Loaders are tools that help you **load data** from various sources and convert it into **Document objects**. These are essential for integrating data from external systems like Slack, Notion, Google Drive, and many more into LangChain workflows.


 **How Document Loaders Work**

1. **Connect to a Data Source**: Each loader is designed for a specific type of source (e.g., CSV files, APIs, databases).
2. **Load Data**: Use the `.load()` method to fetch and transform data into **Document objects**.
3. **Use Documents**: Once loaded, these documents can be processed or analyzed in your LangChain application.


**Key Features**

| **Feature**              | **What It Does**                                  | **Why It’s Useful**                              |
|--------------------------|--------------------------------------------------|------------------------------------------------|
| **Source-Specific Loaders** | Works with hundreds of data sources like Slack, Notion, and Google Drive. | Makes it easy to bring in data from multiple platforms. |
| **Consistent API**        | All loaders use the `.load()` method.            | Simple and standardized usage.                 |
| **Flexible Parameters**   | Each loader has source-specific configuration options. | Customize it for your data needs.              |


**Example: Loading Data from a CSV File**

```python
from langchain_community.document_loaders.csv_loader import CSVLoader

# Initialize the loader with specific parameters (e.g., file path)
loader = CSVLoader(file_path="data.csv")

# Load the data
documents = loader.load()

# Access loaded documents
for doc in documents:
    print(doc.page_content)
    print(doc.metadata)
```



 **Why Use Document Loaders?**

1. **Integrate Data Easily**: Supports a wide variety of data sources.
2. **Streamlined Workflow**: Automatically converts raw data into LangChain-compatible Document objects.
3. **Standardized Approach**: Consistent `.load()` method across different loaders.



**Getting Started**
- **Pick the Right Loader**: Choose a loader for your specific data source (e.g., CSVLoader for CSV files, SlackLoader for Slack messages).
- **Set Parameters**: Configure the loader with source-specific options (e.g., file paths, API keys).
- **Load and Process Data**: Use the `.load()` method to fetch and work with your data.

---

This beginner-friendly explanation simplifies the concept while highlighting how ChatHistory works in LangChain and why it’s useful.
