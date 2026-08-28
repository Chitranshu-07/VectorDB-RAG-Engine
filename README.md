\# VectorDB RAG Engine



A C++17-based vector database and Retrieval-Augmented Generation (RAG) system that combines custom vector search algorithms with local AI models through Ollama.



The project supports Brute Force, KD-Tree, and HNSW vector search, document chunking, semantic retrieval, and local LLM-based answer generation.



\## Features



\- Custom Vector Database implemented in C++

\- Brute Force nearest-neighbor search

\- KD-Tree vector search

\- HNSW approximate nearest-neighbor search

\- Euclidean, Cosine, and Manhattan distance metrics

\- Document chunking

\- Semantic document search

\- Local text embeddings using `nomic-embed-text`

\- Local LLM generation using `llama3.2`

\- Retrieval-Augmented Generation (RAG)

\- REST API using `cpp-httplib`

\- Browser-based frontend

\- Document insertion and deletion

\- Local execution without cloud AI APIs



\## Architecture



```text

&#x20;                   Browser

&#x20;                      |

&#x20;                      v

&#x20;               +--------------+

&#x20;               |  index.html  |

&#x20;               +------+-------+

&#x20;                      |

&#x20;                    HTTP

&#x20;                      |

&#x20;                      v

&#x20;               +--------------+

&#x20;               | C++ Backend  |

&#x20;               |  REST Server |

&#x20;               +------+-------+

&#x20;                      |

&#x20;         +------------+-------------+

&#x20;         |                          |

&#x20;         v                          v

&#x20;   Vector Database              RAG Pipeline

&#x20;         |                          |

&#x20;   +-----+-----+                    |

&#x20;   |     |     |                    |

&#x20;   v     v     v                    v

&#x20; Brute  KD    HNSW             Text Chunking

&#x20; Force  Tree                       |

&#x20;                                   v

&#x20;                           Ollama Embeddings

&#x20;                          nomic-embed-text

&#x20;                                   |

&#x20;                                   v

&#x20;                             Vector Search

&#x20;                                   |

&#x20;                                   v

&#x20;                            Relevant Chunks

&#x20;                                   |

&#x20;                                   v

&#x20;                               llama3.2

&#x20;                                   |

&#x20;                                   v

&#x20;                             Final Answer



RAG Pipeline



The document question-answering pipeline works as follows:



Document

&#x20;  |

&#x20;  v

Text Chunking

&#x20;  |

&#x20;  v

nomic-embed-text

&#x20;  |

&#x20;  v

768-Dimensional Embedding

&#x20;  |

&#x20;  v

DocumentDB

&#x20;  |

&#x20;  v

Vector Search

&#x20;  |

&#x20;  v

Top-K Relevant Chunks

&#x20;  |

&#x20;  v

Prompt Construction

&#x20;  |

&#x20;  v

llama3.2

&#x20;  |

&#x20;  v

Generated Answer



Vector Search Algorithms

Brute Force



Brute Force compares the query vector directly against stored vectors.



Query

&#x20; |

&#x20; +--> Vector 1

&#x20; +--> Vector 2

&#x20; +--> Vector 3

&#x20; +--> ...

&#x20; +--> Vector N



It provides a simple exact-search baseline.



KD-Tree



KD-Tree organizes vectors using a tree-based spatial partitioning approach.



It demonstrates a traditional indexing approach for multidimensional data.



HNSW



HNSW (Hierarchical Navigable Small World) uses a graph-based structure for approximate nearest-neighbor search.



The document-search implementation switches to HNSW once the stored document collection reaches the configured threshold.



Distance Metrics



The VectorDB supports:



Euclidean distance

Cosine distance

Manhattan distance



Cosine distance is used by the document semantic-search pipeline.



Ollama



The project uses Ollama to run the AI models locally.



Embedding Model

nomic-embed-text



This model converts text into numerical vector embeddings.



The current embedding output used by the project is 768-dimensional.



Generation Model

llama3.2



This model generates the final answer using the retrieved document context.



REST API

Insert Document

POST /doc/insert



Example:



{

&#x20; "title": "Binary Tree",

&#x20; "text": "A binary tree is a tree data structure..."

}



The server chunks the text, generates embeddings, and stores the chunks in the VectorDB.



Semantic Search

POST /doc/search



Example:



{

&#x20; "question": "What is a binary tree?",

&#x20; "k": 3

}



Returns the most relevant document chunks.



RAG Question Answering

POST /doc/ask



Example:



{

&#x20; "question": "What is a binary tree?",

&#x20; "k": 3

}



The endpoint performs:



Question

&#x20;  |

Embedding

&#x20;  |

Vector Search

&#x20;  |

Relevant Context

&#x20;  |

llama3.2

&#x20;  |

Answer

List Documents

GET /doc/list



Returns stored document chunks and metadata.



Delete Document

DELETE /doc/delete/:id



Example:



DELETE /doc/delete/1

System Status

GET /status



Returns:



Ollama availability

Embedding model

Generation model

Document count

Embedding dimensions

Statistics

GET /stats



Returns VectorDB statistics and supported algorithms and metrics.



Tech Stack

Technology	Purpose

C++17	VectorDB and backend

cpp-httplib	HTTP server/client

HNSW	Approximate nearest-neighbor search

KD-Tree	Vector indexing

Brute Force	Exact-search baseline

Ollama	Local AI runtime

nomic-embed-text	Text embeddings

llama3.2	LLM generation

HTML	Frontend

CSS	Frontend styling

JavaScript	Frontend logic



Project Structure

VectorDB-RAG-Engine/

|

├── main.cpp

├── httplib.h

├── index.html

├── README.md

└── .gitignore



Build files, temporary test files, and local IDE configuration are excluded through .gitignore.



Requirements

Windows

C++17 compatible compiler

MinGW / GCC

Ollama

Required Ollama models

Setup

1\. Clone the repository

git clone https://github.com/YOUR\_USERNAME/VectorDB-RAG-Engine.git

cd VectorDB-RAG-Engine

2\. Install Ollama



Install Ollama and verify:



ollama --version

3\. Download the models

ollama pull nomic-embed-text

ollama pull llama3.2



Verify:



ollama list

Build



Compile the project using:



g++ -std=c++17 main.cpp -o main.exe -lws2\_32

Run



Start the server:



./main.exe



The application will be available at:



http://localhost:8080



Open that address in a browser.



Testing

Check system status

Invoke-RestMethod -Uri "http://localhost:8080/status"

Insert a document

Invoke-RestMethod -Uri "http://localhost:8080/doc/insert" `

&#x20; -Method POST `

&#x20; -ContentType "application/json" `

&#x20; -Body '{"title":"Binary Tree","text":"A binary tree is a tree data structure where each node has at most two children."}'

Search

Invoke-RestMethod -Uri "http://localhost:8080/doc/search" `

&#x20; -Method POST `

&#x20; -ContentType "application/json" `

&#x20; -Body '{"question":"What is a binary tree?","k":3}'

Ask a RAG question

Invoke-RestMethod -Uri "http://localhost:8080/doc/ask" `

&#x20; -Method POST `

&#x20; -ContentType "application/json" `

&#x20; -Body '{"question":"What is a binary tree?","k":3}'

Example

User Question

&#x20;    |

&#x20;    v

"What is a binary tree?"

&#x20;    |

&#x20;    v

nomic-embed-text

&#x20;    |

&#x20;    v

Query Embedding

&#x20;    |

&#x20;    v

Vector Search

&#x20;    |

&#x20;    v

Relevant Document Chunk

&#x20;    |

&#x20;    v

llama3.2

&#x20;    |

&#x20;    v

Generated Answer

Current Limitations

Vector data is currently stored in memory.

Restarting the server clears the document database.

Ollama must be installed and running locally.

The current deployment is local rather than publicly hosted.

Production-level authentication and security are not implemented.

Large-scale performance benchmarking is planned as a future improvement.

Future Improvements

Persistent vector storage

Larger-scale performance benchmarking

Recall@K evaluation

Configurable HNSW parameters

Batch document ingestion

Improved text chunking

Concurrent query processing

API authentication and security

Docker support

Cloud deployment

Author

Chitranshu Kumar



Computer Science Engineering



Interests:



C++

Data Structures and Algorithms

Artificial Intelligence

Machine Learning

Vector Databases

Retrieval-Augmented Generation

Software Development

License



This project is intended for educational and personal use.

