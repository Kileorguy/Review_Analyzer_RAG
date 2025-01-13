# Review_Analyzer_RAG
This is a project that leverages Retrievel-Augmented Generation (RAG) to analyze and ummarize user reviews on instagram. 

The dataset is collected from : https://www.kaggle.com/datasets/saloni1712/instagram-play-store-reviews

![Flow](https://github.com/Kileorguy/Review_Analyzer_RAG/blob/main/Documentation/RAG%20Flow.png?raw=true)

## Installation

This guide demonstrates how to install the library using Anaconda. Feel free to use another tool if you prefer. Feel free to change the environment name too if you prefer, in this case I will be using "Review_Analyzer"

1. Open Command Prompt from the root folder 

2. Create an Anaconda environment with Python 3.10.x:  
   ```bash
   conda create -n Review_Analyzer python=3.10
3. Activate the anaconda environment
    ```bash
   conda activate Review_Analyzer
4. Install the libraries based on requirements.txt
    ```bash
    pip install -r requirements.txt
## Flow
- Read CSV Data
- Split CSV data into chunks
- Embed using Open AI Embedding
- Store to Vector DB
- Query
- Generate 5 more questions with LLM - Query Translation
- Get docs from vector db using the query from Query Translation
- Merge the relevant context and remove duplicates
- Generate Final Prompt using the relevant context and original question

## Detailed Explanation
This section explains the code in the file main.ipynb. The file app.py contains the same logic but has been modified to run on Flask, providing an HTML interface.

The following explanations are based on the headers in the notebook’s markdown.

### Storing to Vector DB (ChromaDB)

In this part of the code, some exploratory data analysis (EDA) is performed. First, the dataset is read, and the value counts of the ratings are checked. The code also verifies whether there are any null or duplicate values. The findings indicate that there are no null or duplicate values, so the process continues. Next, the code checks the token length of each row. This information is used to determine the chunk size and overlap settings. The results vary depending on the number of rows selected.

### Query Translation

In this section, a prompt template is created, as shown below:

`You are an AI language model assistant. Your task is to generate five different versions of the given user question to retrieve relevant documents from a vector database. By generating multiple perspectives on the user question, your goal is to help the user overcome some of the limitations of the distance-based similarity search. Please focus on the clarity of the question and add more details to it. Provide these alternative questions separated by newlines. Original question: {query}`

The use of this is to make five alternative questions for more clarity and specific that can be used later for retrieving data from VectorDB. 

 

### Get Documents from Vector DB.

There is one function “process_multiple_queries”. Where this connect with retriever so it is able to get the result of documents from chromadb using similarity search and only take the unique documents based on the function “get_only_unique”. 

### Retrieval

In this step, the data retrieved from the vector database is merged so it can be used in the generation or main prompting part.
### Generation

In the code, a prompt template is created like shown below : 

`You are an AI assistant for question-answering tasks. Use the following pieces of retrieved context and information to answer the question. If you don't know the answer, say that you don't know. If the data is not relevant to the question, don't use the data. Make sure to give a detailed answer. Context: {context} Question: {query}`

The use of this prompt template is so when the model is not sure about the answer it will say they don’t know and if the context is not really relevant just ignore it. 

- Context is all of the unique documents that is generated from the Query Translation and then used for searching in VectorDB
- Query is the original user query.

## Website Demo
![Image](https://github.com/Kileorguy/Review_Analyzer_RAG/blob/main/Documentation/Website.png?raw=true)
[Demo Video Here](https://www.youtube.com/watch?v=qA8o_w6GvJg)



