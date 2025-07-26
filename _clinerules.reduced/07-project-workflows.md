# Project Workflows

This document provides structured, step-by-step workflows for common types of personal projects. These workflows are designed to be followed in conjunction with the other `clinerules`.

## 1. Web Scraping Workflow

**Objective**: To build a simple, polite, and robust web scraper.

-   **Stage 1: Exploration (Manual)**
    1.  Manually visit the target website to understand its structure.
    2.  Use browser developer tools (F12) to inspect the HTML of the elements you want to scrape.
    3.  Check the website's `robots.txt` file (e.g., `www.example.com/robots.txt`) for scraping rules.

-   **Stage 2: Prototyping (in a Notebook)**
    1.  Use a Jupyter notebook (`notebooks/scraper_prototype.ipynb`) to test your logic.
    2.  Fetch a single page using `requests`.
    3.  Parse the page with `BeautifulSoup` and experiment with selectors (`find`, `find_all`) to extract the target data.

-   **Stage 3: Building the Scraper (in `src`)**
    1.  Create a new file: `src/scrapers/my_scraper.py`.
    2.  **Version 1 (Make it Work)**: Write a function that takes a URL, fetches the page, and parses the data, based on your notebook prototype.
    3.  **Version 2 (Make it Safe)**: Add `try...except` blocks for network errors. Add a `time.sleep(1)` delay between requests to be polite. Set a `User-Agent` header.
    4.  **Version 3 (Make it Flexible)**: Refactor to save the data to a CSV or JSON file in the `data/output` directory. Add parameters to your function to allow for scraping multiple pages.

## 2. Data Analysis Workflow

**Objective**: To explore, clean, and derive insights from a dataset.

-   **Stage 1: Exploration (in a Notebook)**
    1.  Place your raw data in the `data/input` directory.
    2.  In a new notebook (`notebooks/data_exploration.ipynb`), load the data into a pandas DataFrame.
    3.  Use `.head()`, `.info()`, `.describe()`, and `.isnull().sum()` to get a first look at the data's structure, types, and quality.

-   **Stage 2: Cleaning Pipeline (in `src`)**
    1.  Create a new file: `src/data/cleaning_pipeline.py`.
    2.  Write functions to handle specific cleaning tasks identified in your exploration (e.g., `remove_duplicates`, `fill_missing_values`, `correct_data_types`).
    3.  Chain these functions together into a single `clean_data(df)` function that returns a cleaned DataFrame.

-   **Stage 3: Analysis and Visualization (in `src` or Notebook)**
    1.  Using the cleaned data, perform your analysis.
    2.  Calculate summary statistics, group data using `.groupby()`, and find correlations with `.corr()`.
    3.  Create simple plots using `matplotlib` or `seaborn` to visualize your findings. Save plots to the `data/output` directory.

## 3. Automation Script Workflow

**Objective**: To automate a repetitive manual task.

-   **Stage 1: Define the Task**
    1.  Clearly write down the exact, manual steps of the task you want to automate.
    2.  Identify the inputs (e.g., files in a folder) and the desired outputs (e.g., renamed files, a summary email).

-   **Stage 2: Build the Core Logic**
    1.  Create a new file: `scripts/my_automation_script.py`.
    2.  Write a function that performs the core action (e.g., `organize_files_by_type`, `fetch_daily_report`).
    3.  Focus on making the logic work for a single, simple case first. Use `print` statements to show what the script is doing.

-   **Stage 3: Add Safety and Configuration**
    1.  Add robust error handling to manage unexpected situations (e.g., files not found, network issues).
    2.  Add a "dry run" mode. Use a flag (e.g., `dry_run=True`) that, when active, causes the script to print what it *would* do without actually doing it (e.g., printing "Would move file X to folder Y" instead of moving it).
    3.  Move any hardcoded values (like file paths or API keys) into a configuration file or environment variables.
