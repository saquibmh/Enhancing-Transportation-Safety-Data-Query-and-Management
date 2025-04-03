# README

## Project Overview
This project is structured into two main components:

1. **Agentic AI on FARS Data and Arizona AADT Data**
2. **Retrieval-Augmented Generation (RAG) on Arizona Cash Facts**

---

## Arizona Crash Facts Analysis with RAG
### **Objective**
Analyze the "Arizona Crash Facts 2023" PDF document using Retrieval-Augmented Generation (RAG). This section demonstrates multiple methods for extracting and querying traffic accident data in Arizona.

### **Methods**
1. **Direct PDF Text Extraction:**
   - Extracts raw text from the PDF using `pypdf`.
   - Uses `TextLoader` and `RecursiveCharacterTextSplitter` for processing.
   - Stores embeddings with `Annoy` for retrieval.
   - Queries are processed via OpenAI's GPT-4o-mini.
   
2. **Table Separation with Document Intelligence:**
   - Pre-processes the PDF to separate tables and text.
   - Indexes structured and unstructured data separately.
   - Retrieves information using a RAG pipeline.

3. **Table Summarization with LLM:**
   - Summarizes tables and text with GPT-4.
   - Combines structured and summarized content for indexing.
   - Enhances responses using a RAG pipeline.

4. **Table Summarization with LLM and Table Context:**
   - Uses pre-generated summaries alongside table data.
   - Combines retrieved tables and summaries to provide accurate insights.
   
### **Installation & Setup**
```bash
# Install dependencies
pip install langchain-huggingface langchain_community annoy pypdf
```

### **Usage**
```bash
# Run the RAG pipeline on Arizona Crash Facts
python crash_facts_rag.py --query "What are the top causes of accidents in 2023?"
```

---

## Agentic AI on AADT Data Implementation

### **Notebook Setup**
The project includes a Jupyter Notebook implementation that installs dependencies and processes multiple Excel files containing AADT data:

```python
%%capture
!pip install pandas openpyxl langchain python-dotenv langchain_openai langchain_experimental tabulate

from dotenv import load_dotenv
load_dotenv()
```

### **Processing AADT Data**
AADT data is stored in multiple Excel files and read using Pandas:

```python
import pandas as pd

root_file_path = "data/"
file_path = ["2017-aadt-publication.xlsm", "2018-AADT-PUBLICATION.xlsx", "2019-AADT-PUBLICATION.xlsx",
             "2020-AADT-PUBLICATION.xlsx", "2021-AADT-PUBLICATION.xlsx", "2022-AADT-PUBLICATION.xlsx",
             "2023-AADT-PUBLICATION.xlsx"]

final_df = []

for idx, file in enumerate(file_path):
  if idx < len(file_path) - 2:
    df = pd.read_excel(root_file_path + file, skiprows=4, sheet_name='ALL MAINLINE SECTIONS')
  else:
    df = pd.read_excel(root_file_path + file, skiprows=4)
  final_df.append(df)
```

### **Defining the AI Model and Agent**

```python
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(temperature=0, model="gpt-4o-mini")

from langchain_experimental.agents.agent_toolkits import create_pandas_dataframe_agent
from langchain.agents.agent_types import AgentType

agent = create_pandas_dataframe_agent(
    chat,
    final_df,
    verbose=True,
    agent_type=AgentType.OPENAI_FUNCTIONS,
    allow_dangerous_code=True,
    prefix="\nYou are working with a pandas dataframe in Python. The name of the dataframe is `df`. Make sure you do every possible thing to answer the user's query. If still you feel there might be some issue with the query, let the user know along with a small solve.",
)
```

### **Querying the Agent**
Example of querying the agent for AADT data:

```python
# Filter for relevant exits and year 2021
exit_7_aadt_2021 = df[(df['Start'].str.contains('Exit 7 Araby Rd')) & (df['End'].str.contains('Exit 3 SR 280 / Avenue 3E'))]['AADT 2021']

# Get the AADT value
exit_7_aadt_2021_value = exit_7_aadt_2021.values
print(exit_7_aadt_2021_value)
```

---

## Contributing
1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Commit changes (`git commit -m 'Added new feature'`).
4. Push to the branch (`git push origin feature-branch`).
5. Open a Pull Request.

---

## Contact
For any questions or suggestions, open an issue or reach out via email at `saquib@arizona.edu`. 

