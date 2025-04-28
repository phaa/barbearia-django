# BarberPro

This project consists of a Python + Django system developed for the Corporate Systems course during the Internet Systems undergraduate program at IFRN Campus Canguaretama. The main goal of this application is to assist local barbers with scheduling their clients, aiming to reduce waiting lines and the no-show rate due to prolonged wait times. Due to the short development period of the application, we chose to use a CSS template that we adapted to our needs using Gulp to compile the SCSS.

## Project Screenshots

<p align="center">
 <img src="https://github.com/phaa/barbearia-django/blob/main/imagens-projeto/login.png" title="Screen" width="800" />
</p>

<p align="center">
 <img src="https://github.com/phaa/barbearia-django/blob/main/imagens-projeto/cadastro.png" title="Screen" width="800" />
</p>

<p align="center">
 <img src="https://github.com/phaa/barbearia-django/blob/main/imagens-projeto/minhas-barbearias.png" title="Screen" width="800" />
</p>

<p align="center">
 <img src="https://github.com/phaa/barbearia-django/blob/main/imagens-projeto/cadastro-barbearia.png" title="Screen" width="800" />
</p>

<p align="center">
 <img src="https://github.com/phaa/barbearia-django/blob/main/imagens-projeto/overview-barbearia.png" title="Screen" width="800" />
</p>

<p align="center">
 <img src="https://github.com/phaa/barbearia-django/blob/main/imagens-projeto/overview-barbearia-2.png" title="Screen" width="800" />
</p>

<p align="center">
 <img src="https://github.com/phaa/barbearia-django/blob/main/imagens-projeto/agendamento.png" title="Screen" width="800" />
</p>


## Running with Docker

This project can be easily executed using Docker and Docker Compose. Ensure you have Docker and Docker Compose installed on your machine.

**Prerequisites:**

* [Docker](https://www.docker.com/get-started) installed.
* [Docker Compose](https://docs.docker.com/compose/install/) installed.

**Steps to Run:**

1.  **Navigate to the project's root directory** in your terminal.

2.  **Create the `.env` file:** Make sure you have a `.env` file in your project root with the necessary configurations.

3.  **Execute Docker Compose:** Use the following command to build the Docker image and start the containers.

    ```bash
    $ docker-compose up -d --build
    ```

4.  **Access the Application:** The application will be available at `http://localhost:85`.

**Docker Files Structure:**

* **`Dockerfile`:** Instructions to build the Django application's Docker image.
    * Base: `python:3.9`.
    * Copies the code.
    * Sets environment variables.
    * Installs dependencies (`requirements.txt`).
    * Runs Django migrations.
    * Starts Gunicorn.

* **`docker-compose.yml`:** Defines and manages the Docker containers.
    * **`services`:**
        * **`appseed-app`:** Django container (built from the `Dockerfile`, restarts on failure, reads `.env`, connected to `db_network` and `web_network`).
        * **`nginx`:** Nginx web server (uses `nginx:latest` image, maps port `85:85`, mounts `./nginx:/etc/nginx/conf.d`, connected to `web_network`, depends on `appseed-app`).
    * **`networks`:**
        * `db_network`: Network for database communication.
        * `web_network`: Network for communication between Nginx and the Django application.

With this setup, the BarberPro project can be run in a Dockerized environment.

## Running without Docker

Follow these instructions to set up and run the project in your local environment:

1.  **Get the Code:** Unzip the source files or clone the repository from GitHub.

    ```bash
    $ # Get the code
    $ git clone https://github.com/phaa/barbearia-django.git
    $ cd barbearia-django
    ```

2.  **Set Up a Virtual Environment (Recommended):** Isolate the project dependencies in a virtual environment.

    * **On Unix/macOS based systems:**

        ```bash
        $ python3 -m venv env
        $ source env/bin/activate
        ```

    * **On Windows based systems:**

        ```bash
        $ python -m venv env
        $ .\env\Scripts\activate
        ```

3.  **Install Dependencies:** Use the `requirements.txt` file to install the necessary libraries.

    ```bash
    $ pip install -r requirements.txt
    ```

4.  **Create and Apply Migrations:** Prepare the database by creating the tables defined in the Django models.

    ```bash
    $ python manage.py makemigrations
    $ python manage.py migrate
    ```

5.  **Start the Development Server:** Run the application in development mode.

    * **Using the default port (8000):**

        ```bash
        $ python manage.py runserver
        ```

    * **Using a custom port (replace `<your_port>` with the desired port number):**

        ```bash
        $ python manage.py runserver 0.0.0.0:<your_port>
        ```

6.  **Access the Application:** Open your web browser and navigate to `http://127.0.0.1:8000/` (or the custom port you set).

> **Note:** To start using the system, access the registration page and create a new user account. After authentication, all application features will be accessible.

<br />

## Codebase Structure

The project's structure has been organized in a clear and intuitive manner:
```bash
<PROJECT_ROOT>
├── core/                                # Main configurations for the Django application
│   ├── settings.py                      # Defines the global settings for the project
│   ├── wsgi.py                          # Entry point for WSGI servers (production)
│   └── urls.py                          # Defines the URL patterns for the entire project
│
├── apps/
│   ├── home/                            # Simple application to serve basic HTML pages
│   │   ├── views.py                     # Logic to display pages for authenticated users
│   │   └── urls.py                      # Defines specific URLs for this application
│   │
│   ├── authentication/                  # Handles user authentication (login and registration)
│   │   ├── urls.py                      # Defines URLs related to authentication
│   │   ├── views.py                     # Logic for user login and registration
│   │   └── forms.py                     # Defines forms for login and registration
│   │
│   ├── static/                          # Static files (CSS, JavaScript, images)
│   │   └── assets/                      # Directory for assets (mentioned in CSS recompilation)
│   │       └── &lt;css, JS, images>       # CSS files, JavaScript files, and images
│   │
│   ├── templates/                       # HTML template files
│   │   ├── includes/                    # Reusable HTML fragments and components
│   │   │   ├── navigation.html          # Top navigation menu component
│   │   │   ├── sidebar.html             # Sidebar navigation component
│   │   │   ├── footer.html              # Application footer
│   │   │   └── scripts.html             # JavaScript scripts common to all pages
│   │   │
│   │   ├── layouts/                     # Base templates for pages
│   │   │   ├── base-fullscreen.html    # Layout used by full-screen pages (e.g., authentication)
│   │   │   └── base.html                # Default layout for system pages
│   │   │
│   │   ├── accounts/                    # Pages related to authentication
│   │   │   ├── login.html               # Login page
│   │   │   └── register.html            # Registration page
│   │   │
│   │   └── home/                        # Example UI Kit pages
│   │       ├── index.html               # System homepage
│   │       ├── 404-page.html            # 404 error page
│   │       └── *.html                   # Other interface pages
│   │
├── requirements.txt                     # List of project dependencies (usually includes the database driver)
├── .env                                 # File for environment variables (sensitive configurations)
└── manage.py                            # Django's utility script for project management
```

<br />

> **Initialization Flow:**
>
> * The `manage.py` script initializes Django and uses the settings defined in the `core/settings.py` file.
> * `core/settings.py` loads additional configurations from the `.env` file (if present).
> * Unauthenticated users are automatically redirected to the login page.
> * Upon successful authentication, access to the application pages (`apps/`) is granted.

<br />

## Recompiling CSS (If Applicable)

The instructions below are for recompiling SCSS files, in case the project uses this methodology to generate the CSS files. If the project does not use SCSS, this section can be omitted or adapted.

<br />

**Step #1 - Install Tools:**

* Make sure you have [Node.js](https://nodejs.org/en/) (version 12.x or higher) installed on your system.
* Install [Gulp](https://gulpjs.com/) globally using npm (Node Package Manager):

```bash
$ npm install -g gulp-cli
 ```

<br />

**Step #2 - Navigate to the Assets Directory:**

```bash
$ cd apps/static/assets
```

**Step #3 - Install JS modules **

```bash
$ npm install
```

**Step #4 - Edit and Recompile SCSS Files:**
```bash
$ gulp scss
```
> The generated CSS file (usually named after the theme or a main file like style.css) will be saved in the static/assets/css/ directory.
