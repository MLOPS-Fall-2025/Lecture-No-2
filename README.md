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

git clone https://github.com/your-username/flask-app.git
cd flask-app


Create a new branch for the feature

git checkout -b feature/user-login


Make changes & commit

`git add .`

`git commit -m "Added user login form"`


Push the branch to GitHub

`git push origin feature/user-login`


Open a Pull Request (PR)

Go to GitHub → Your repo → "Pull requests" → "New pull request".

Compare feature/user-login → main.

Request review → Merge when tests pass.

