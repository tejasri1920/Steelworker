# ADR 002: Technology Stack Selection

**Status:** Accepted

## Context
The project requires heavy data manipulation (Assumption 1) and rapid dashboard generation for a non-technical audience.

## Decision: Stack A (Python + Streamlit)
We will use the following "Data-First" stack:
* **Language:** Python 3.x
* **UI/Dashboard:** Streamlit
* **Data Engine:** Pandas (for Excel reconciliation)
* **Database/ORM:** SQLAlchemy + SQLite

## Comparison against Dimensions

| Dimension | Decision (Stack A) | Rationale |
| :--- | :--- | :--- |
| **AI Support** | **Highest** | Native access to Python-based LLM libraries for future growth. |
| **Popularity** | **High** | Strong community support for both Quality Engineering and Python. |
| **Ecosystem** | **Best for Data** | Pandas is the industry standard for cleaning inconsistent Excel data. |
| **Deployment** | **Simplest** | Can be deployed as a single container or internal script. |
| **Next Arch.** | **Seamless** | Easy to migrate to a Data Lake or cloud-hosted Snowflake/Databricks environment. |

## Alternatives Considered
* **Stack B (Spring Boot):** Too much boilerplate for simple dashboarding.
* **Stack C (Node.js):** Lacks the robust data-science libraries needed for statistical trend analysis.

## Consequences
* **Positive:** Rapid prototyping; "Quality Engineer" friendly language.
* **Negative:** Streamlit's "State" management can be tricky for complex multi-page interactions.
