This plan outlines the final development phase to transition your **FoodHub AI Chatbot** from an interim SQL Agent to a production-ready **Multi-Tool Chat Agent**. As a Principal Engineer, this transition focuses on modularity, security-first design, and closing the feedback loop from initial baseline failures.

### ---

**Phase 1: Environment Setup & Data Augmentation (The Foundation)**

**Objective:** Bridge local development in Cursor with Google Colab and expand the dataset to handle complex edge cases.

* **Cursor Development Steps:**  
  1. **Sync Environment:** Download customer\_orders.db and your current logic to a local folder in Cursor.  
  2. **Security:** Create a .env file for your OPENAI\_API\_KEY to prevent accidental exposure.  
  3. **Local Composition:** Use Cursor’s "Composer" feature to refactor the interim notebook code into a clean .py structure for tool definitions.  
* **Synthetic Data Generation:**  
  1. **Surge Scenario:** Inject 50+ rows with identical timestamps to test prioritization.  
  2. **Edge Case Generation:** Specifically add rows with NULL or None values for delivery\_eta and delivery\_time to test the bot's ability to communicate uncertainty.  
  3. **Customer Cohorting:** Add a cust\_tier column (e.g., "Gold", "Standard") to allow for prioritized formal responses in the **Answer Tool**.  
* **Closing Metric:** Database expanded to \>100 rows with 100% schema integrity across five distinct order\_status types.

### ---

**Phase 2: Multi-Tool Architecture Development (The Core)**

**Objective:** Implement the discrete tools required by the final rubric to separate data retrieval from response refinement.

* **Order Query Tool Implementation:**  
  1. **Functionality:** Wraps the hardened SQL logic.  
  2. **ID Pinning V2:** The tool must now accept cust\_id as a mandatory input parameter to prevent the "Broad Data Leakage" found in interim testing.  
* **Answer Tool (Persona Refinement) Implementation:**  
  1. **Feedback Integration:** Use "Few-Shot" prompting to teach the bot to apologize for cancellations rather than saying "I don't know".  
  2. **Format Masking:** Ensure technical terms like "NULL" or "Dataframe" are never shown to the user.  
* **Closing Metric:** 100% pass rate on unit tests for the **Answer Tool** mapping raw database "NULL" fields to empathetic user phrases.

### ---

**Phase 3: Agent Orchestration & Safety Guardrails (The Brain)**

**Objective:** Combine tools into a Chat Agent with advanced input/output filtering.

* **Closing the Feedback Loop (Safety):**  
  1. **Pre-Processor Guardrail:** Implement a keyword-based semantic monitor to detect medical/allergy terms *before* the SQL tool is called.  
  2. **Constraint Refinement:** Update the system prompt to explicitly block the "Hacker" role-play scenario identified in the rubric.  
* **Agent Definition:** Use LangChain to initialize the **Chat Agent** with access to both the Query and Answer tools.  
* **Closing Metric:** Successful block of a "Hacker" prompt and 100% trigger rate for "Allergic Reaction" escalation.

### ---

**Phase 4: Interactive Interface & mandatory Testing (The UI)**

**Objective:** Build the functional loop and answer the required evaluation queries.

* **chatagent() Implementation:**  
  1. **Interactive Loop:** Build a Python function that manages a while loop for continuous user interaction.  
  2. **Stateful Memory:** Implement ConversationBufferWindowMemory so the user doesn't have to repeat their Order ID during a support session.  
* **Mandatory Scenario Execution:** Run the four specific queries defined in the problem statement:  
  1. Hacker Attempt.  
  2. Frustrated User/Multiple Queries.  
  3. Cancellation Request.  
  4. Vague "Where is my order?" query.  
* **Closing Metric:** Zero crashes across all four mandatory test scenarios with verbose logs showing the tool-calling flow.

### ---

**Phase 5: Final Documentation & Business Reporting (The Polish)**

**Objective:** Finalize the HTML Notebook and PDF Business Report based on rubric checklists.

* **Notebook Comments:** Add markdown cells explaining the data flow from **User → Chat Agent → Order Query Tool → Answer Tool**.  
* **Business Report (PDF):**  
  1. **KPI Analysis:** Finalize the projected **70% cost reduction** metric based on test success rates.  
  2. **Actionable Insights:** Include recommendations for **Predictive Support** (e.g., the bot proactively checking for kitchen delays).  
* **Closing Metric:** Final rubric self-audit score of 70/70 points.

**Principal Engineer's Tip for Cursor:** Use Cursor’s "Cmd+L" to ask for a "Rubric Audit" on your final Python functions. It can identify if you missed a sub-requirement, such as the polite/formal tone requirement for the Answer Tool.