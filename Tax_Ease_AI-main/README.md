
# **TaxEase AI: Your AI-Powered Tax Assistant**

**TaxEase AI** is an advanced conversational assistant designed to streamline the tax filing process for users. By combining state-of-the-art AI technologies like OpenAI's GPT-4, Pinecone Vector Database, and LangChain, TaxEase AI provides personalized, real-time assistance in navigating tax forms, retrieving documents, and identifying potential deductions and credits.

---
# [🚀 TaxEase AI Chatbot Deployment](https://taxeaseai-fb4rgyfyxb8yksawyhhfvx.streamlit.app/#taxease-ai-chatbot)
Access the live deployment of the TaxEase AI Chatbot by clicking the link above.



## **Technologies Used**

### Core Technologies
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)
![Pinecone](https://img.shields.io/badge/Pinecone-005BFF?style=for-the-badge&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-009688?style=for-the-badge&logoColor=white)
![PyPDF](https://img.shields.io/badge/PyPDF-8C001A?style=for-the-badge&logo=adobeacrobatreader&logoColor=white)

### Auxiliary Technologies
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)
![JSONL](https://img.shields.io/badge/JSONL-FFB100?style=for-the-badge)

---

## **Project Overview**

TaxEase AI is a user-centric, conversational assistant that empowers individuals with simplified, AI-driven tax guidance. This project enables users to:

- **Understand tax regulations** via conversational responses.
- **Retrieve relevant tax documents** using a Vector Database.
- **Generate personalized insights** by fine-tuning a large language model (LLM) for tax-specific scenarios.
- **Simplify complex processes** like document preprocessing, data embedding, and response generation.

---

## **Problem Statement**

Tax filing is inherently complex due to:
- The need to analyze extensive tax regulations and forms.
- Limited tools for personalized assistance in tax document management.
- Inefficiency in retrieving relevant tax-specific information.

TaxEase AI addresses these pain points by providing:
- **Automated document preprocessing** using tools like PyPDF.
- **Accurate and real-time assistance** via a fine-tuned LLM.
- **Seamless document retrieval** powered by Pinecone Vector Database.

---

## **Key Features**

### 1. **Streamlit-Based User Interface**
- **User-Friendly Chatbot**: Users can input queries and receive tax-related guidance.
- **Clear Conversations**: Chat history is displayed in an intuitive layout.

### 2. **Document Preprocessing with PyPDF**
- Extracts and preprocesses data from tax forms (PDF).
- Converts extracted data into JSONL format for further embedding and fine-tuning.

### 3. **Embedding Generation**
- Text data is chunked into manageable sizes.
- Embeddings are generated using OpenAI’s models for similarity-based document retrieval.

### 4. **Pinecone Vector Database**
- Efficiently stores vectorized embeddings.
- Provides fast retrieval of relevant tax documents during a query.

### 5. **LangChain for Document Retrieval**
- Handles indexing and real-time retrieval of tax documents.
- Provides contextual information for personalized responses.

### 6. **Fine-Tuned LLM**
- GPT-4 fine-tuned on tax-specific data.
- Generates detailed, conversational responses tailored to the user’s input.

---

## **Architecture Diagram**

![AI-Powered Tax Assistant Architecture](https://github.com/Poornachandra77/Tax_Ease_AI/blob/main/architecture%20design%20(1).png)

---

# **Detailed Workflow**

The implementation of the TaxEase AI system comprises three major components, each integral to achieving the system's objective of providing accurate, user-friendly tax assistance.

---

## **1. Fine-Tuning the LLM**

To ensure that the language model is well-suited for handling tax-related queries, the GPT-2 model was fine-tuned using a domain-specific JSONL dataset containing prompts (questions) and completions (answers). This process involved:

### **1.1 Dataset Preparation**
- **Data Source**:  
  The dataset was constructed by extracting tax-related questions and answers from authoritative sources like IRS forms, guidelines, and online resources.
- **Format**:  
  The data was structured in JSONL format, where each line represents a single prompt-completion pair:
  ```json
  {"prompt": "What is the tax filing deadline?", "completion": "The tax filing deadline is April 15th."}
### **1.2 Tokenization**
- Tokenization converts textual data into numerical tokens that the model can process. This was achieved using Hugging Face's `AutoTokenizer`, which is pre-configured for the GPT-2 model.
- Padding and truncation were applied to ensure uniform input lengths, with a maximum sequence length of 512 tokens.

---

### **1.3 Fine-Tuning Process**

#### **Framework**
- The Hugging Face `Trainer` API was used for fine-tuning. This framework simplifies the process by integrating PyTorch training loops with automatic optimization.

#### **Training Details**
- **Optimizer**: AdamW optimizer was used for weight updates.  
- **Learning Rate**: Set to 2e-5 for gradual adjustments to model weights.  
- **Batch Size**: A batch size of 8 was used, balancing GPU memory constraints and efficient training.  
- **Epochs**: The model was trained over 3 epochs to achieve convergence without overfitting.  

#### **Hardware Acceleration**
- Fine-tuning was conducted on an NVIDIA T4 GPU, leveraging PyTorch’s CUDA support for accelerated computation.

---

### **1.4 Loss Metrics**

#### **Training Loss**
- The model achieved a training loss of 3.48, indicating that it successfully minimized errors during training.

#### **Validation Loss**
- A validation loss of 0.09 was recorded, demonstrating that the model generalizes well to unseen data.
### **2. Retrieval-Augmented Generation (RAG) Pipeline**

To ensure accurate and contextually relevant responses, the RAG pipeline was implemented, integrating a retrieval system with the language model.

---

### **2.1 Embedding Generation**

#### **Model**
- OpenAI’s `text-embedding-ada-002` was used to generate 1536-dimensional embeddings for tax-related documents and user queries.

#### **Chunking**
- Long documents were split into smaller chunks (up to 8192 tokens) to ensure they fit within the model’s context window.

#### **Storage**
- The embeddings, along with metadata, were stored in the Pinecone vector database for efficient retrieval.

---

### **2.2 Pinecone Vector Search**
- The vector database indexes document embeddings and retrieves the top-k most relevant chunks based on cosine similarity with the query embedding.
- Retrieved chunks serve as additional context for the language model.

---

### **2.3 Fine-Tuned GPT-2**
- The fine-tuned GPT-2 model processes the user query along with the retrieved context to generate a coherent and accurate response.
### **Pipeline Workflow**

---

### **Step-by-Step Process**

1. **User Query**  
   - The user enters a tax-related question in the Streamlit app.

2. **Embedding Generation**  
   - The query is converted into an embedding vector using OpenAI’s embedding model.

3. **Document Retrieval**  
   - Pinecone retrieves the most relevant tax-related documents based on the query embedding.

4. **Response Generation**  
   - The fine-tuned GPT-2 model generates a natural language response using the retrieved context.

5. **Response Delivery**  
   - The chatbot displays the response in the Streamlit interface.

---


## **Setup Instructions**

# **Setup Instructions for TaxEase AI**

Follow these detailed instructions to set up and run the TaxEase AI system locally or on your preferred cloud platform.

---

## **1. Prerequisites**

### **1.1 Software Requirements**
- **Python**: Version 3.8 or above.
- **pip**: Latest version for Python package installation.
- **Git**: For cloning the repository.

### **1.2 Hardware Requirements**
- **GPU**: Recommended for faster training and fine-tuning (e.g., NVIDIA T4 or above).
- **Memory**: Minimum 16GB RAM.

---

## **2. Clone the Repository**

1. Open your terminal or command prompt.
2. Clone the repository:
   ```bash
   git clone https://github.com/Poornachandra77/Tax_Ease_AI.git
   
### **Navigate to the Project Directory**
cd Tax_Ease_AI

## **3. Install Dependencies**

---

### **3.1 Create a Virtual Environment (Optional but Recommended)**

#### **Create a Virtual Environment**

python -m venv env

Activate the Virtual Environment

On Windows:
.\env\Scripts\activate

On macOS/Linux:
source env/bin/activate


## **4. Configure API Keys**

---

### **4.1 OpenAI API Key**
- Add your OpenAI API key in the `.streamlit/secrets.toml` file:
   ```toml
   OpenAI_Key = "your_openai_api_key"

 ### **4.2 Pinecone API Key
Add your Pinecone API key in the .streamlit/secrets.toml file:

Pinecone_Key = "your_pinecone_api_key"

## ** Run the Application**

---

### **6.1 Start the Streamlit App**
1. Run the Streamlit application:
   streamlit run taxeaseai.py

   Open the provided local or network URL in your browser (e.g., http://localhost:8501).




