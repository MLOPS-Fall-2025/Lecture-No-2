# Lecture-No-2
## Tutorial: Branching & CI Pipelines in GitHub
### Introduction

When working in a team, everyone contributes code simultaneously. If all developers directly push changes to the main branch, it can quickly become chaotic.

- Branches allow developers to work on features or fixes independently before merging them back into main.

- CI pipelines (Continuous Integration) automatically build and test your code whenever changes are pushed, ensuring nothing breaks before merging.

### Example Scenario

Imagine you’re working on a Flask web app hosted on GitHub.

The main branch contains the production-ready app.

A new feature "Add user login" needs to be developed.

While you’re coding, GitHub Actions should automatically run tests to ensure your changes don’t break existing functionality.

### Branching Workflow

Clone the repository

``git clone https://github.com/your-username/flask-app.git``

`cd flask-app`


Create a new branch for the feature

`git checkout -b feature/user-login`


Make changes & commit

`git add .`

`git commit -m "Added user login form"`


Push the branch to GitHub

`git push origin feature/user-login`


Open a Pull Request (PR)

Go to GitHub → Your repo → "Pull requests" → "New pull request".

Compare feature/user-login → main.

Request review → Merge when tests pass.

### CI Pipeline with GitHub Actions

Let’s set up a GitHub Actions workflow to run tests on every push or PR.

- Create file: .github/workflows/ci.yml

```
name: CI Pipeline

on:
  push:
    branches:
      - main
      - feature/*
  pull_request:
    branches:
      - main

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        run: pytest
```

### Exercises
- Exercise 1: Branching Basics

    - Create a new branch called feature/add-about-page.

    - Add an about.html file in your Flask app.

    - Commit & push changes.

    - Open a PR to merge into main.

- Exercise 2: Breaking the Build

     - Intentionally introduce a bug (e.g., wrong import in app.py).

    - Push changes to your feature branch.

    - Observe GitHub Actions fail.

    - Fix the bug and push again.

    - Confirm the pipeline turns green.

- Exercise 3: Enforcing CI in Pull Requests

    - Enable branch protection on main (Settings → Branches → Protect main).

    - Require that status checks must pass before merging.

    - Try merging a PR with failing tests → notice it’s blocked.

- Exercise 4: Extend the Pipeline

    - Add a flake8 step in the CI pipeline for linting.

    - Push code with a style violation.

    - Fix and confirm the pipeline passes again.

### Key Takeaways

- Branches isolate work, prevent conflicts, and make collaboration smooth.

- Pull Requests allow for code review and safe merging.

- CI Pipelines automatically test and validate changes, ensuring quality.

- Branch protection rules enforce CI before merging to main.

### Flask minimal application
Here’s a minimal Flask app with a simple login page. This example uses just Flask and HTML templates (no database yet, credentials are hardcoded for simplicity).

```
flask-login-app/
│
├── app.py
├── templates/
│   ├── login.html
│   └── welcome.html
```

### app.py

```
from flask import Flask, render_template, request, redirect, url_for, flash, session

app = Flask(__name__)
app.secret_key = "your_secret_key"  # Required for sessions & flash messages

# Hardcoded credentials for demo
USERNAME = "admin"
PASSWORD = "password123"

@app.route("/")
def home():
    if "user" in session:
        return redirect(url_for("welcome"))
    return redirect(url_for("login"))

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form["username"]
        password = request.form["password"]

        if username == USERNAME and password == PASSWORD:
            session["user"] = username
            flash("Login successful!", "success")
            return redirect(url_for("welcome"))
        else:
            flash("Invalid credentials. Try again.", "danger")

    return render_template("login.html")

@app.route("/welcome")
def welcome():
    if "user" not in session:
        flash("Please log in first.", "warning")
        return redirect(url_for("login"))
    return render_template("welcome.html", user=session["user"])

@app.route("/logout")
def logout():
    session.pop("user", None)
    flash("You have been logged out.", "info")
    return redirect(url_for("login"))

if __name__ == "__main__":
    app.run(debug=True)
```

### templates/login.html

```
<!DOCTYPE html>
<html>
<head>
    <title>Login Page</title>
</head>
<body>
    <h2>Login</h2>

    {% with messages = get_flashed_messages(with_categories=true) %}
      {% if messages %}
        <ul>
        {% for category, message in messages %}
          <li><strong>{{ category.title() }}:</strong> {{ message }}</li>
        {% endfor %}
        </ul>
      {% endif %}
    {% endwith %}

    <form method="POST">
        <label>Username:</label><br>
        <input type="text" name="username" required><br><br>

        <label>Password:</label><br>
        <input type="password" name="password" required><br><br>

        <button type="submit">Login</button>
    </form>
</body>
</html>
```

### templates/welcome.html

```
<!DOCTYPE html>
<html>
<head>
    <title>Welcome</title>
</head>
<body>
    <h2>Welcome, {{ user }} 🎉</h2>
    <a href="{{ url_for('logout') }}">Logout</a>
</body>
</html>
```