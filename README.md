# Overview
This project aimed to develop a Retrieval Augmented Generation (RAG) based chatbot capable of accurately answering questions from a life insurance policy document. The system was designed to be robust and efficient, leveraging the power of large language models (LLMs) and semantic search.

#Design
The system architecture consists of three core layers:
•	Embedding Layer: This layer processes the PDF document, cleans the text, and divides it into chunks for embedding. Chunking was performed at the page level. Embeddings were generated using OpenAI's text-embedding-ada-002 model.
•	Search Layer: The search layer involves designing user queries and embedding them using the same model as the document chunks. These query embeddings are then used to search a ChromaDB vector database containing the document chunk embeddings. A cache mechanism is implemented to store previously searched queries and their results, enabling faster retrieval for repeated queries. Re-ranking of search results is performed using a cross-encoder model (cross-encoder/ms-marco-MiniLM-L-6-v2) to enhance relevance.
•	Generation Layer: This layer focuses on designing an effective prompt for the LLM (GPT 3.5). The prompt instructs the LLM to utilize the retrieved document chunks and user query to generate a concise answer, extract relevant information (including tabular data), and provide citations from the source document.

#Implementation
The implementation of the chatbot involves several key components:
•	Data Processing: The pdfplumber library was used to extract text and tables from the PDF document. Pages with less than 10 words were filtered out. Each page was treated as a separate chunk, and metadata (policy name and page number) was stored for each chunk.
•	Embedding and Storage: OpenAI's embedding model was used to generate embeddings for all chunks. ChromaDB was used as the vector database to store the embeddings and associated metadata.
•	Semantic Search: User queries were embedded and searched against the ChromaDB collection. A cache mechanism using a separate ChromaDB collection was implemented to store and retrieve previous query results, improving efficiency.
•	Re-ranking: The cross-encoder/ms-marco-MiniLM-L-6-v2 model was employed to re-rank the search results based on their relevance to the user query.
•	Response Generation: GPT 3.5 was used to generate the final answer. A detailed prompt was designed to guide the LLM in utilizing the retrieved information, extracting relevant details, formatting the answer, and providing citations.
