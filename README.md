## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### AIM:
To design and implement a LangChain Expression Language (LCEL) expression that utilizes at least two prompt parameters and three key components (prompt, model, and output parser), and to evaluate its functionality by analyzing relevant examples of its application in real-world scenarios.

### PROBLEM STATEMENT:
Design and implement LangChain Expression Language (LCEL) using Simple Chain and Complex Chain to process user queries and generate meaningful answers by combining prompt templates, language models, output parsers, and document retrieval techniques.

### DESIGN STEPS:

#### STEP 1: Setup API and Environment: Load environment variables using dotenv and set openai.api_key from the local environment.

#### STEP 2:Create Prompt and Model: Use LangChain to define a ChatPromptTemplate and initialize ChatOpenAI for text generation.

#### STEP 3:Build a Retrieval System: Store predefined texts in DocArrayInMemorySearch with OpenAIEmbeddings and create a retriever.

#### STEP 4: Define Question-Answering Chain: Use RunnableMap to fetch relevant documents and pass them to a chat model for responses.

#### STEP 5: Invoke the Chain: Run chain.invoke() with a question to retrieve context-based answers using the LangChain pipeline.

### PROGRAM:
Name: Megala M S
Reg NO: 212225040230

Simple Chain
```

from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser

prompt = ChatPromptTemplate.from_template(
    "Explain about {topic} in simple words"
)

model = ChatOpenAI(
    model_name="gpt-4o-mini",
    temperature=0
)

output_parser = StrOutputParser()

chain = prompt | model | output_parser

chain.invoke({"topic": "Python Programming"})
```
Complex Chain

```
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import DocArrayInMemorySearch
from langchain.schema.runnable import RunnableMap
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import DocArrayInMemorySearch
from langchain.schema.runnable import RunnableMap

vectorstore = DocArrayInMemorySearch.from_texts(
    [
        "Python is a programming language used for web development and automation.",
        "Machine Learning is a branch of Artificial Intelligence that enables computers to learn from data.",
        "Cloud Computing provides servers, storage, and databases over the internet."
    ],
    embedding=OpenAIEmbeddings()
)

retriever = vectorstore.as_retriever()

template = """Answer the question based only on the following context:
​
{context}
​
Question: {question}
"""
​
prompt = ChatPromptTemplate.from_template(template)

chain = RunnableMap({
    "context": lambda x: retriever.get_relevant_documents(
        x["question"]
    ),
    "question": lambda x: x["question"]
}) | prompt | model | output_parser
chain.invoke({
    "question": "What is Machine Learning?"
})
chain.invoke({
    "question": "What is Machine Learning?"
})
```
### OUTPUT:

Simple Chain
<img width="741" height="532" alt="image" src="https://github.com/user-attachments/assets/17500927-7927-4290-b073-20ef2ffc4d78" />


Complex Chain
<img width="731" height="54" alt="image" src="https://github.com/user-attachments/assets/b88a7e91-2bc8-4080-a345-fd2dda1e6e0c" />

### RESULT:
