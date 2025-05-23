# Bloom's Taxonomy Learning Objective Classifier

This project uses AI, specifically LangChain and OpenAI's language models, to classify learning objectives based on Bloom's Taxonomy. It analyzes course materials and learning objectives to determine the cognitive complexity required.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/kbandesz/QGenAI/blob/main/QGenAI_test.ipynb)

## Project Purpose

The primary goal of this project is to automate the classification of learning objectives using Bloom's Taxonomy. By leveraging Natural Language Processing (NLP) and Large Language Models (LLMs), this tool aims to:

- Assist instructional designers and educators in quickly assessing the cognitive level of learning objectives.
- Ensure that learning objectives are aligned with the desired cognitive skills.
- Provide a consistent framework for curriculum analysis and development.

## How it Works

The project operates through a Jupyter Notebook (`QGenAI_test.ipynb`) and utilizes the LangChain framework to interact with an OpenAI language model (e.g., GPT-4). The process is as follows:

1.  **Course Material Loading**: The script loads course content from `.docx` files. Each module or distinct section of course material should be in a separate `.docx` file.
2.  **Learning Objective Input**: Learning objectives are read from `.txt` files. Each objective should be on a new line within the text file.
3.  **Prompt Engineering**: A detailed prompt template guides the language model. This template includes:
    *   The course material.
    *   The specific learning objective to classify.
    *   Definitions and examples for each level of Bloom's Taxonomy (Remember, Understand, Apply, Analyze, Evaluate, Create).
4.  **AI-Powered Classification**: The language model analyzes the learning objective in the context of the provided course material and Bloom's Taxonomy definitions.
5.  **JSON Output**: For each learning objective, the system outputs a JSON object containing:
    *   `bloom_level_1`: The primary Bloom's Taxonomy level identified.
    *   `rationale`: An explanation for the classification.
    *   `ambiguous`: A flag ('Yes' or 'No') indicating if the objective could potentially fit into a second Bloom's level.
    *   `ambiguous_reason`: If `ambiguous` is 'Yes', a reason for the potential alternative classification.
    *   `bloom_level_2`: The alternative Bloom's level, if applicable.

## Setup Instructions

To run this project, you'll need Python and Jupyter Notebook installed on your system.

1.  **Clone the Repository (if applicable)**:
    ```bash
    # If you have this project in a git repository, clone it:
    # git clone <repository-url>
    # cd <repository-directory>
    ```

2.  **Set Up OpenAI API Key**:
    The script requires an OpenAI API key. This is typically set as an environment variable or directly within the notebook if you are using Google Colab's `userdata` feature.
    *   **Local Environment**: Set an environment variable named `OPENAI_API_KEY` with your key.
        ```bash
        export OPENAI_API_KEY='your_openai_api_key_here'
        ```
        (For Windows, use `set OPENAI_API_KEY=your_openai_api_key_here` in Command Prompt or set it through System Properties.)
    *   **Google Colab**: The notebook is pre-configured to fetch the API key using `userdata.get('OPENAI_API_KEY')`. You will need to add your API key as a secret in Colab (View -> Secrets -> Add new secret).

3.  **Install Dependencies**:
    The project requires several Python packages. You can install them using pip:
    ```bash
    pip install langchain langchain-openai langchain-community docx2txt
    ```

4.  **Organize Input Files**:
    *   Place your course material `.docx` files in the project's working directory (or a subdirectory). The notebook currently expects files named like `MPC_Module_1.docx`, `MPC_Module_2.docx`, etc.
    *   Place your learning objectives `.txt` files in the same directory. Each learning objective should be on a new line. The notebook expects corresponding files like `MPC_Module_1_Obj.txt`, `MPC_Module_2_Obj.txt`, etc.
    *   Update the file paths in the `QGenAI_test.ipynb` notebook if your file names or directory structure differ. Specifically, check the `load_course_material` and `read_learning_objectives` calls.

## Usage

1.  **Open the Jupyter Notebook**:
    Launch Jupyter Notebook and open `QGenAI_test.ipynb`.
    ```bash
    jupyter notebook QGenAI_test.ipynb
    ```
    Alternatively, if using Google Colab, upload and open the notebook there.

2.  **Configure API Key and File Paths (if needed)**:
    *   Ensure your OpenAI API key is correctly set up as per the "Setup Instructions".
    *   Verify that the paths to your course material (`.docx`) and learning objective (`.txt`) files are correctly specified in the notebook cells. The default script iterates through files named `MPC_Module_1.docx`, `MPC_Module_1_Obj.txt`, etc.

3.  **Run the Notebook Cells**:
    Execute the cells in the notebook sequentially.
    *   The initial cells handle environment setup, package imports, and LLM initialization.
    *   The main processing loop loads data, invokes the LangChain chain for each learning objective, and prints the results.

4.  **Interpret the Output**:
    For each learning objective, the script will print:
    *   The learning objective itself.
    *   The determined primary Bloom's Level.
    *   The rationale behind the classification.
    *   Whether the classification is considered ambiguous.
    *   If ambiguous, the reason and the alternative Bloom's Level.

    Example output for a single learning objective:
    ```
    Learning Objective 1/4: "Summarize the historical context of institutional policy communication."

    Determined Bloom's Level (Primary): Understand

    Rationale: The learning objective requires learners to explain the historical context of institutional policy communication, which involves interpreting and conveying information in their own words. This aligns with the 'Understand' level of Bloom's Taxonomy, as it goes beyond mere recall and requires comprehension of the concepts presented in the course materials.
    Is Classification Ambiguous?: No

    --------------------------------------
    ```

## Customization

You can customize the project in several ways:

*   **Language Model**: The script is initialized with `model = "gpt-4o-mini"`. You can change this to other OpenAI models compatible with the ChatOpenAI class (e.g., `"gpt-4"`, `"gpt-3.5-turbo"`). Ensure the chosen model supports the required token limit and provides good results for this task.
*   **Prompt Template**: The core logic is driven by the `prompt_template` defined in the notebook. You can modify this template to:
    *   Adjust the system message (e.g., change the persona of the AI).
    *   Refine the definitions or examples of Bloom's Taxonomy levels.
    *   Change the structure or content of the expected JSON output (if you also update the `JsonOutputParser` and downstream code accordingly).
*   **Course Material and Objectives**: Adapt the script to read files from different locations or with different naming conventions by modifying the `load_course_material` and `read_learning_objectives` functions, and the main processing loop.
*   **Temperature and `max_tokens`**: In the `llm = ChatOpenAI(...)` initialization, you can adjust:
    *   `temperature`: Controls the randomness of the output. A lower value (like 0.0) makes the output more deterministic.
    *   `max_tokens`: The maximum number of tokens the model can generate for its response.
