# Personalized Daily News Summarizer

**Author:** Vivek Patel (Vivekcoder07)
**License:** Proprietary - All Rights Reserved (See [LICENSE](LICENSE) file for full details)

A Flask-based web application that scrapes news from public websites, summarizes articles using Natural Language Processing (NLP), and provides a personalized news feed based on user interests and interactions.

## Core Features

*   **News Aggregation:** Scrapes articles from multiple pre-configured news sources (e.g., BBC, Reuters, NPR, AP News - *AP News feed currently experiencing 404 errors, investigation pending*).
*   **Automated Content Processing:**
    *   Downloads full article text.
    *   Calculates estimated reading time.
    *   Generates concise extractive summaries (2-3 lines) of articles.
*   **User Personalization:**
    *   User registration and login system.
    *   Users can select multiple topics of interest (e.g., Technology, Sports, World News).
    *   Personalized news dashboard displaying articles relevant to selected interests.
*   **Reading Management:**
    *   **Mark as Read/Unread:** Users can mark articles as read or unread.
    *   Visual indicators for read articles (faded appearance, checkmark icon).
    *   Clicking "Read Original" automatically marks an article as read.
*   **Bookmarking:** Users can bookmark articles for later reading (currently session-based, user-specific DB persistence planned).
*   **Dynamic UI:**
    *   Modern, responsive user interface built with Bootstrap 5 and custom CSS.
    *   Glassmorphism and Aurora background effects for a contemporary look.
    *   Curved design elements for a softer aesthetic.
    *   Interactive elements like animated skeleton loaders and scroll-triggered card animations.
*   **Background Tasks:**
    *   Automated daily fetching of new articles.
    *   Automated summarization of newly fetched articles.
    *   Uses a scheduler to run these tasks periodically.

## Project Structure

```
/Personalized-Daily-News-Summarizer
|-- /app                     # Main application package
|   |-- /auth                # Authentication blueprints, forms, routes
|   |-- /data_access         # SQLAlchemy models (Article, User, UserInterest, UserReadArticle)
|   |-- /news_processing     # Scraping (fetch.py) and summarization (summarize.py) logic
|   |-- /personalization     # Filtering logic for personalized content (filters.py)
|   |-- /routes              # Main application routes (main_routes.py)
|   |-- /static              # CSS, JavaScript, images
|   |-- /templates           # HTML templates (using Jinja2)
|   |-- __init__.py          # Application factory (create_app)
|   |-- config.py            # Application configuration (news sources, API keys - via .env)
|   |-- extensions.py        # Flask extension initializations (db, migrate, login_manager, etc.)
|-- /instance                # Instance-specific configuration (e.g., SQLite DB file, logs) - NOT version controlled
|-- /migrations              # Alembic database migration scripts
|-- run.py                   # Main script to run the Flask development server
|-- requirements.txt         # Python dependencies
|-- .gitignore               # Specifies intentionally untracked files that Git should ignore
|-- LICENSE                  # Project license (All Rights Reserved)
|-- README.md                # This file
# Optional: .env for environment variables (copy from .env.example if provided)
```

## Setup and Installation

1.  **Prerequisites:**
    *   Python 3.10 or higher.
    *   `pip` (Python package installer).
    *   Git (for cloning).

2.  **Clone the Repository:**
    ```bash
    git clone <your_repository_url>  # Replace with your actual GitHub repository URL
    cd Personalized-Daily-News-Summarizer
    ```

3.  **Create and Activate a Virtual Environment:**
    ```bash
    # Windows
    python -m venv venv
    venv\Scripts\activate

    # macOS/Linux
    python3 -m venv venv
    source venv/bin/activate
    ```

4.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *   **NLTK Resource:** The `newspaper3k` library (used for scraping) requires the `punkt` tokenizer models from NLTK. If you encounter issues, you might need to download it manually:
        ```python
        # Run this in a Python interpreter within your activated virtual environment
        import nltk
        nltk.download('punkt')
        ```

5.  **Environment Variables:**
    *   Create a `.env` file in the project root directory.
    *   Add the following, at a minimum:
        ```env
        FLASK_APP=run.py
        FLASK_DEBUG=True  # Set to False in production
        SECRET_KEY='your_strong_random_secret_key'  # Generate a secure random key
        DATABASE_URL=sqlite:///../instance/news_app.db # Path to your SQLite database
        # Optional: Add API keys here if any external services are integrated
        ```
    *   The `instance` folder and `news_app.db` will be created automatically if they don't exist.

6.  **Initialize the Database & Run Migrations:**
    *   Ensure your `DATABASE_URL` in `.env` is correctly configured.
    *   The application uses Flask-Migrate (Alembic) for database schema management.
    ```bash
    # Initialize migrations (only if the migrations folder doesn't exist yet - rarely needed if already set up)
    # flask db init 

    # Create an initial migration (if starting from scratch and models are defined)
    # flask db migrate -m "Initial database setup"

    # Apply migrations to create/update database tables
    flask db upgrade
    ```
    The `instance` folder will be created by Flask if it doesn't exist. The database file (`news_app.db`) will be created inside `instance/` as per the `DATABASE_URL`.

## Running the Application

1.  **Start the Flask Development Server:**
    ```bash
    flask run
    ```
    The application will typically be available at `http://127.0.0.1:5000/`.

2.  **Background Tasks (News Fetching & Summarization):**
    The application is configured to run news fetching and summarization tasks in background threads upon startup and then periodically (default: every 24 hours). Check the application logs for the status of these tasks.

## Usage Flow

1.  **Register/Login:** Access the application and register a new user account or log in if you have one.
2.  **Set Preferences:** Navigate to the "Preferences" page to select your news topics of interest.
3.  **View Dashboard:** Go to your "Dashboard" to see a personalized feed of news articles with summaries.
4.  **Interact with Articles:**
    *   Click on an article title or "Read Original" to open the source article in a new tab (this will also mark it as read).
    *   Use the eye icon (<i class="fas fa-eye"></i> / <i class="fas fa-eye-slash"></i>) to manually toggle the read status.
    *   Use the bookmark icon (<i class="far fa-bookmark"></i> / <i class="fas fa-bookmark"></i>) to save articles.
5.  **View Bookmarks:** Access your saved articles on the "Bookmarks" page.

## Key Technologies & Libraries

*   **Backend Framework:** Flask
*   **Programming Language:** Python
*   **Database ORM:** Flask-SQLAlchemy
*   **Database Migrations:** Flask-Migrate (Alembic)
*   **Authentication:** Flask-Login, Werkzeug (for password hashing)
*   **Web Forms:** Flask-WTF
*   **News Scraping/Parsing:** `newspaper3k`, `feedparser`
*   **NLP (Summarization):** (Assumed to be `sumy` or similar, not explicitly in recent logs but typical for this setup - *please verify and update based on actual summarization library if different*)
*   **Scheduling (Internal):** Python `threading` and `schedule` library for background tasks.
*   **Frontend:** HTML5, CSS3, JavaScript (ES6+)
*   **UI Framework:** Bootstrap 5
*   **Templating Engine:** Jinja2

## Current Status & Known Issues

*   **AP News Feed:** The AP News RSS feed URLs configured seem to be returning 404 errors. These URLs may need to be updated or an alternative found.
*   **Error Handling:** While basic error handling is in place, more comprehensive error logging and user feedback for background tasks (scraping, summarization) can be improved.
*   **Bookmark Persistence:** Bookmarks are currently session-based. For full persistence, they should be stored in the database linked to user accounts (similar to `UserReadArticle`).

*This README was last updated on May 28, 2025, by an AI assistant based on project analysis.* Please review and customize it further to accurately reflect your project details and future vision. 
