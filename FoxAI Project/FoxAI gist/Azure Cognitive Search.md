
https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/azure-cognitive-search-and-langchain-a-seamless-integration-for/ba-p/3901448
## What is vector search?

Vector search is a capability for indexing, storing, and retrieving vector embeddings from a search index

You can use vector search to power similarity search, multi-modal search, recommendation engines, or applications implementing the [Retrieval Augmented Generation (RAG) architecture](https://aka.ms/what-is-rag).


1. Retrieve source documents  from the data source.
2. Chunk your Data.
3. Azure OpenAI embedding model.
4. Add a vector field in your index defination in cognitive search.
5. Load the index.

your solution must contain calls to an embedding model to create the embeddings before you save them to an index, you need to also call the same embedding model to vectorize your search query before sending it to Cognitive Search

  
Here is the order you need to follow to perform the pure vector or hybrid search queries.

1. Once the user submits the query in the client application, call the same [Azure OpenAI embedding model](https://learn.microsoft.com/azure/ai-services/openai/how-to/embeddings) (or other embedding model) used to create the vector embeddings that were saved initially in the index.
2. [Submit the vector or hybrid query](https://learn.microsoft.com/azure/search/search-get-started-vector#run-queries) to your Cognitive Search index


![[Pasted image 20240914111150.png]]


## What is LangChain?

[LangChain](https://docs.langchain.com/docs/) is a framework for developing applications powered by language models. It allows you to connect a language model to other sources of data, interact with its environment, and create sequences of calls to achieve specific tasks.

## Getting started with Azure Cognitive Search in LangChain

The [Azure Cognitive Search LangChain integration](https://python.langchain.com/docs/integrations/vectorstores/azuresearch), built in Python, provides the ability to chunk the documents, seamlessly connect an embedding model for document vectorization, store the vectorized contents in a predefined index, perform similarity search (pure vector), hybrid search and hybrid with semantic search. It also provides configurability to create your own index and apply scoring profiles to achieve better search accuracy.