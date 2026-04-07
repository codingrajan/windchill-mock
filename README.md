# PTC x Plural Windchill Mock Exam Platform 🚀

A high-performance, fully client-side mock examination platform designed specifically for PLM professionals preparing for the **PTC Certified Windchill Implementation Practitioner** certification. 

Built with a decoupled architecture, this platform dynamically fetches question banks, executes algorithmic difficulty routing, and provides a highly realistic, blind-testing environment complete with advanced post-exam analytics.

---

## 📑 Table of Contents
1. [Overview](#-overview)
2. [Key Features](#-key-features)
3. [Architecture & Tech Stack](#-architecture--tech-stack)
4. [Directory Structure](#-directory-structure)
5. [Setup & Deployment](#-setup--deployment)
6. [Question Bank Management (JSON Schema)](#-question-bank-management-json-schema)
7. [Exam Generation Algorithm](#-exam-generation-algorithm)
8. [Maintenance & Updates](#-maintenance--updates)

---

## 📖 Overview

The Windchill Implementation Practitioner exam requires deep domain knowledge across multiple PLM concepts. This platform simulates the actual exam experience by evaluating users on core domains such as:
* Product Lifecycle Management (PLM) fundamentals and Windchill architecture.
* Foundational object models (WTParts, CAD documents, EPMDocuments).
* Business administration, contexts, and object initialization rules (OIRs).
* Change and configuration management (Lifecycles, Workflows, Change Notices).
* System administration, server tuning, and properties management.

---

## ✨ Key Features

* **Dynamic Question Pooling:** Automatically utilizes JavaScript `Promise.all` to fetch and compile up to 500 questions from multiple decoupled JSON banks seamlessly.
* **Configurable Exam Parameters:**
  * **25 Questions** (15-minute timer)
  * **50 Questions** (35-minute timer)
  * **75 Questions** (60-minute timer)
  * **100 Questions** (75-minute timer)
* **Algorithmic Difficulty Routing:** Compiles exams based on strict distribution percentages across *Easy*, *Medium*, and *Difficult* parameters to ensure balanced testing.
* **Blind Assessment Environment:** Mimics strict exam conditions. Selections are tracked and can be toggled, but correct answers and explanations are entirely hidden until final submission.
* **Advanced Analytics Dashboard:** * Calculates the strict **80% passing threshold**.
  * Evaluates sectional performance to identify the user's **Strongest** and **Weakest** domains.
  * Generates actionable study recommendations based on the weakest performance area.
* **Detailed Review Interface:** Provides a post-exam, question-by-question breakdown mapping user selections against correct answers, accompanied by deep-dive, manual-cited explanations.

---

## 🛠 Architecture & Tech Stack

This platform is a **Single Page Application (SPA)** that runs entirely in the browser. It requires zero backend infrastructure or databases.

* **Frontend:** HTML5, CSS3 (Custom responsive UI)
* **Logic:** Vanilla JavaScript (ES6+ async/await, DOM manipulation)
* **Data Storage:** Static JSON files (Decoupled question banks)
* **Hosting:** GitHub Pages (Recommended)

---

## 📂 Directory Structure

For the platform to function correctly, your repository must adhere to this exact flat-file structure:

```text
/
├── index.html                  # Main application UI and JavaScript logic
├── ptc_logo.png                # Header UI graphic (Ensure exact filename)
├── plural_logo.jpg             # Header UI graphic (Ensure exact filename)
├── windchill_mock_test_1.json  # Question Bank 1 (Seed data)
├── windchill_mock_test_2.json  # Question Bank 2 (Add as developed)
├── windchill_mock_test_3.json  # Question Bank 3 (Add as developed)
├── windchill_mock_test_4.json  # Question Bank 4 (Add as developed)
└── windchill_mock_test_5.json  # Question Bank 5 (Add as developed)
```
## Setup & Deployment
⚠️ Important Note on Local Testing: Because this application uses the fetch() API to load external JSON files, opening index.html directly from your hard drive (file:///) will trigger a CORS (Cross-Origin Resource Sharing) block in modern browsers.

To run this platform, it must be hosted on a web server. The easiest and free method is GitHub Pages.

**Deployment via GitHub Pages (Recommended)**
* Initialize a new public repository on your GitHub account.
* Upload the files listed in the Directory Structure directly to the root of the repository.
* Navigate to the repository Settings.
* In the left sidebar, click Pages.
* Under "Build and deployment", set the Source to Deploy from a branch.
* Select your main (or master) branch, select / (root), and click Save.
* Within 1-2 minutes, GitHub will provision a live URL (e.g., https://[your-username].github.io/[repo-name]/).

## 📝 Question Bank Management (JSON Schema)
The platform's content is entirely decoupled from its logic. To add, remove, or edit questions, you only need to modify the windchill_mock_test_X.json files.

**Required JSON Object Structure**
Every question object must adhere strictly to the following schema for the platform parser to render it correctly.
```text
{
  "id": 1,
  "topic": "Windchill Architecture",
  "difficulty": "medium", 
  "question": "Which component in the Windchill architecture provides full-text search capability?",
  "options": [
    "The visualization publication worker",
    "The Apache web server",
    "The indexing server",
    "The Oracle database server"
  ],
  "correctAnswer": 2,
  "explanation": "The Windchill Advanced Configuration manual states that the indexing server provides full-text search capability."
}
```
**Schema Rules:**
* id: A unique integer for tracking (internal reference only).
* topic: Must match one of the core exam domains. Note: The analytics engine groups performance dynamically based on exact string matches here.
* difficulty: Must be strictly defined as "easy", "medium", or "difficult". (Crucial for the exam generation algorithm).
* question: The question text (supports basic HTML tags if needed).
* options: An array of exactly 4 strings representing choices A, B, C, and D.
* correctAnswer: Uses 0-based indexing 0 = A, 1 = B, 2 = C, 3 = D.
* explanation: Displayed only during the post-exam review. Should explicitly cite the Windchill manual or PTC best practices.

## 🧠 Exam Generation Algorithm
When a user selects an exam length and clicks "Generate Exam", the JavaScript engine performs the following operations:

1. Mass Fetch: Pulls all available questions from POOL_FILES.
2. Categorization: Sorts the master pool into buckets based on the difficulty key.
3. Distribution Routing: Draws randomly from the buckets based on the selected target size:
   * 25 Questions: 10% Easy | 20% Medium | 70% Difficult
   * 50 Questions: 20% Easy | 40% Medium | 40% Difficult
   * 75 Questions: 25% Easy | 45% Medium | 30% Difficult
   * 100 Questions: 30% Easy | 35% Medium | 35% Difficult
4. Fallback Safety Logic: If the JSON banks lack enough questions of a specific difficulty to fulfill the strict percentage, the algorithm calculates the deficit and dynamically pulls from "unrated" questions, or sequentially from other difficulties, ensuring the exam successfully generates the exact number of requested questions without crashing.
5. Final Shuffle: The finalized subset is shuffled one last time so the order of easy/medium/hard questions is randomized for the user.

## 🔧 Maintenance & Updates
To ensure the platform remains robust and accurate for future PLM practitioners, maintainers should note the following:

* **Syllabus Changes:** If the PTC Certification syllabus domains change, ensure the new domains are used consistently in the topic key of the JSON files. The analytics dashboard will automatically adapt to new topic names.
* **Adding Question Banks:** Currently, the system polls files 1 through 5. If you create a windchill_mock_test_6.json, you must update the POOL_FILES constant array at the top of the JavaScript block in index.html for it to be recognized.
* **Logo Updates:** If branding changes, replace ptc_logo.png and plural_logo.jpg with identically named files, or update the <img> tags in the HTML header.
