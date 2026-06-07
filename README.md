# 📋 Automated Excel Splitting & Distributor Distribution Agent

> [!WARNING]
> **Current Project Status:** This automation workflow is currently under active editing and testing. Emails are temporarily all routing to my own email for validation purposes. The routing workflow will be updated to point to respective distributor emails soon.

## 📖 Project Overview
This project showcases an enterprise-grade cloud automation workflow built using **Microsoft Power Automate**. It eliminates the manual, error-prone task of opening master data files, manually filtering items by channel partners, and cross-docking records individually.

* **The Problem:** Operations teams frequently handle unified master spreadsheets containing multi-distributor records. Manually splitting rows based on distributor names and sending them out runs a high risk of compliance breaches (e.g., sending Distributor A's proprietary data to Distributor B).
* **The Solution:** A scheduled, fully automated cloud flow that reads a dynamic list of distribution channels, programmatically queries rows out of a centralized SharePoint master table, isolates each target's data, converts it to a clean data table (`.csv`), and dispatches it directly to assigned stakeholders.

---

## ⚙️ Core Technical Stack
* **Orchestration:** Microsoft Power Automate (Cloud Flow)
* **Data Source:** Microsoft SharePoint Online (Sales Operations Site)
* **Data Processing Engine:** Excel Online (Business) connector & Data Operations Engine
* **Communication Layer:** Microsoft Outlook 365 Enterprise (`Send an email (V2)`)

---

## 🗺️ System Workflow Architecture

Below is the structured architectural process flow showing how data transforms from the scheduled recurring trigger down to individual file delivery.

### Process Flowchart
![Power Automate Workflow Structure](workflow_chart.png)

### Step-by-Step Execution Breakdown

```mermaid
graph TD
    A[Trigger: Monthly Recurrence] --> B["Initialize variable : Name = Array of Channels, type = Array, value = ['ARROW AEC', 'WPG ASIA INDIA']"]
    B --> C[Apply to Each Channel Loop]
    subgraph Loop [For Each Target Distributor]
        C --> D[List Rows Present in SharePoint Master Table]
        D --> E[Create Isolated CSV Table]
        E --> F[Send Email with Unique CSV Attachment]
    end
    F --> G{Next Channel available?}
    G -- Yes --> C
    G -- No --> H[Workflow Completed Successfully]
