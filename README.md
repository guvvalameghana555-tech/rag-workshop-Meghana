# rag-workshop-Meghana
My RAG system for AI in Healthcare

Why This Topic?

I chose AI in Healthcare because it is one of the most promising and impactful areas where technology directly improves human well-being. The integration of artificial intelligence with medical science allows doctors and researchers to make faster, data-driven decisions, improving accuracy and efficiency in diagnosis and treatment. This topic reflects how innovation can save lives and transform the future of healthcare.

What It Does?

AI in Healthcare uses intelligent algorithms and data analysis to assist medical professionals in diagnosing diseases, predicting health risks, and personalizing treatments. It processes medical images, lab reports, and patient histories to identify patterns and provide insights that might be missed by humans. Overall, it enhances precision, reduces errors, and helps deliver better patient outcomes.

Why RAG (Retrieval-Augmented Generation)?
A standard Large Language Model (LLM) only knows what it was trained on. If you want to ask questions about specific, private, or very recent information (like the documents in my /Data folder), the LLM won't know the answers.

RAG is the perfect solution for this problem. It works in two steps:

1.	Retrieval: When you ask a question (e.g., "What is the history of Wimbledon?"), the system first searches the custom documents (/Data folder) to find the most relevant text snippets.

2.	Generation: It then takes your question and the relevant snippets it found and feeds them to the LLM. It essentially says, "Based on these specific facts, answer this question."

This approach makes the system:
•	Accurate: Answers are grounded in your specific documents.
•	Knowledgeable: It can "talk about" information it was never trained on.
•	Trustworthy: It reduces the chance of the AI "hallucinating" or making up facts.
Core Technologies Used
This project is built primarily with LangChain and Hugging Face, running 100% locally on your machine.
•	transformers / torch (The "Brain"):
o	The transformers library by Hugging Face to load and run the open-source Large Language Model (LLM) locally. AutoModelForCausalLM and AutoTokenizer handle loading the pre-trained model and its tokenizer.
o	torch is the underlying deep learning framework. It's used here to automatically detect and use a GPU (cuda) if available, which dramatically speeds up model inference, or fall back to the CPU (cpu).
•	HuggingFaceEmbeddings (The "Translator"):
o	This is the embeddings model. Its job is to read a piece of text (a document chunk or a question) and convert it into a list of numbers (a "vector"). This vector represents the semantic meaning of the text, not just its keywords.
•	FAISS (The "Memory / Index"):
o	FAISS (Facebook AI Similarity Search) is a high-performance vector store. It takes all the "vectors" from our document chunks and stores them in a database optimized for incredibly fast similarity search.
o	When you ask a question, FAISS finds the document chunks whose vectors (meanings) are closest to your question's vector.
•	PyPDFLoader & RecursiveCharacterTextSplitter (The "Ingestors"):
o	PyPDFLoader: A LangChain utility to load and read the text from all the .pdf files in the /Data folder.
o	RecursiveCharacterTextSplitter: This is crucial. LLMs have a limited "context window" (they can only read so much text at once). This tool breaks down the large documents into small, manageable chunks, which are then easier to embed and retrieve.
•	LangChain (The "Orchestrator"):
o	LangChain is the "glue" that connects all these pieces into a single, seamless pipeline.
o	PromptTemplate: Formats the prompt that is sent to the LLM, ensuring it always includes the user's question and the retrieved context in a consistent way.
o	RunnablePassthrough & StrOutputParser: These are part of the LangChain Expression Language (LCEL), which defines the flow of data. This chain ensures that the retrieved documents, the prompt, and the question are all passed to the model correctly, and the final output is parsed as a clean string.

