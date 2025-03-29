# CrewAI Personalized Learning Path
This project utilizes the CrewAI framework to assemble a team of specialized AI agents capable of generating a personalized learning path for a user based on their goals, existing knowledge, and preferred learning style.

The system takes user input, breaks down the learning objective, assesses knowledge gaps, finds relevant online resources, sequences them logically, and presents a structured plan. This example is designed to run in a Google Colab notebook.

## Features

*   **Personalized Plan Generation:** Creates custom learning roadmaps tailored to individual needs.
*   **Goal Decomposition:** Breaks down high-level learning goals into manageable sub-topics.
*   **Knowledge Gap Assessment:** Identifies areas needing the most focus based on user's self-assessment.
*   **Resource Curation:** Finds relevant online learning materials (tutorials, videos, projects, docs) using web search.
*   **Logical Sequencing:** Arranges topics and resources in a step-by-step, progressive learning order.
*   **Structured Output:** Presents the final plan in a clear, actionable format.
*   **Leverages CrewAI:** Demonstrates multi-agent collaboration for a complex planning task.

## Requirements

*   Python 3.9+
*   Google Colab Environment (Recommended) or a local Python setup.
*   **API Keys:**
    *   **OpenAI API Key:** Essential for the core language model (GPT-3.5/4). Obtain from [platform.openai.com](https://platform.openai.com/). You need a funded account or active trial credits.
    *   **Serper API Key:** Needed for the `ResourceFinder` agent to perform web searches using `SerperDevTool`. Get one from [serper.dev](https://serper.dev/) (a free tier is usually available).

## Setup (Google Colab)

1.  **Get the Notebook:** Clone this repository or download the `.ipynb` notebook file.
2.  **Open in Colab:** Upload and open the notebook in Google Colab ([colab.research.google.com](https://colab.research.google.com/)).
3.  **Install Libraries:** Run the first cell (`# @title 1. Install Necessary Libraries`) to install `crewai`, `crewai-tools`, `langchain-openai`, etc.
4.  **Configure API Keys in Colab Secrets:**
    *   In Colab, click the **Key icon (`<>`)** in the left sidebar ("Secrets").
    *   Enable "Notebook access".
    *   Add two new secrets:
        *   Name: `OPENAI_API_KEY` | Value: Your OpenAI API Key (e.g., `sk-...`)
        *   Name: `SERPER_API_KEY` | Value: Your Serper API Key
    *   **Crucially:** Ensure the **toggle switch** next to *both* secrets is **ON** to grant the notebook permission to access them.
5.  **Run Cell 2:** Execute the second cell (`# @title 2. Import Modules...`). Carefully check its output to confirm that both API keys were found and loaded into the environment variables. Address any errors before proceeding.

## How to Use

1.  **Define User Input (Cell 3):**
    *   Locate the cell titled `# @title 3. Define User Input...`.
    *   Modify the values of the `learning_goal`, `current_knowledge`, and `preferred_style` variables to reflect the specific user you want to generate a plan for. Be descriptive!
2.  **Select LLM (Optional - Cell 4):**
    *   In Cell 4 (`# @title 4. Select LLM...`), you can switch between `gpt-4-turbo` (default, recommended for quality) and `gpt-3.5-turbo` (cheaper) by uncommenting the appropriate `llm = ...` line.
3.  **Run Cells Sequentially:** Execute the remaining cells (4 through 8) in order.
    *   Cell 4 initializes the LLM and tools.
    *   Cell 5 defines the specialized agents.
    *   Cell 6 defines the sequence of tasks for the agents.
    *   **Cell 7** creates the `Crew` and starts the process by calling `kickoff()`. **This is the main execution step.** It will take some time and consume OpenAI API credits. The `verbose=True` setting will show detailed agent activity.
    *   Cell 8 displays the final generated learning plan.

## Workflow Overview

The crew operates sequentially through these agents and tasks:

1.  **Goal Clarifier Agent (Task 1):** Breaks down the main `learning_goal` into key sub-topics.
2.  **Knowledge Assessor Agent (Task 2):** Compares sub-topics to the `current_knowledge` description to identify gaps.
3.  **Resource Finder Agent (Task 3):** Searches the web (using Serper) for resources matching sub-topics and the `preferred_style`.
4.  **Path Planner Agent (Task 4):** Sequences the sub-topics and resources logically, considering prerequisites and knowledge gaps.
5.  **Plan Presenter Agent (Task 5):** Formats the sequence into a final, readable learning plan document.

## Customization

*   **User Inputs:** Change the goal, knowledge description, and style in Cell 3 for different users.
*   **Agent Prompts:** Modify the `role`, `goal`, and `backstory` in Cell 5 to fine-tune agent behavior.
*   **Task Instructions:** Adjust the `description` and `expected_output` in Cell 6 for different levels of detail or specific output requirements.
*   **Resource Finding:** Modify the `ResourceFinder` agent's goal or tools if you want to target specific resource types or databases.
*   **LLM Selection:** Switch LLMs in Cell 4 based on cost/performance trade-offs.

## Limitations & Disclaimer

*   **API Costs:** Using OpenAI models incurs costs based on token usage. Monitor your OpenAI billing dashboard.
*   **Resource Quality & Availability:** The quality and relevance of resources found depend on the search tool and the LLM's ability to evaluate search results. Links may become outdated. Web search can sometimes fail.
*   **Skill Assessment Accuracy:** The assessment relies entirely on the user's self-description in `current_knowledge`.
*   **LLM Reliability:** AI models can occasionally provide inaccurate information or "hallucinate". The generated plan is a suggestion and should be reviewed critically.
*   **Not Professional Advice:** This tool generates suggestions based on AI processing. It is not a substitute for professional educational counseling or expert curriculum design. Use the output as a starting point.
*   **Rate Limits:** You might encounter API rate limits from OpenAI or Serper, especially on free tiers.

