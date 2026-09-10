# github-profile-analyzer

## Project Description

GitHub Profile Analyzer is a Python command-line application that uses the GitHub REST API to fetch and analyze a user's public GitHub profile and repositories.

The project demonstrates Python programming concepts such as variables, conditions, loops, lists, dictionaries, sets, functions, APIs, JSON, and exception handling.

## Features

* Fetch GitHub profile information
* Fetch public repositories
* Display bio, location, company, followers, and following
* Classify developer activity
* Analyze repository languages
* Calculate total stars and forks
* Calculate average stars
* Find the most-starred repository
* Search for a repository
* Search repositories by programming language
* Handle API and network errors

## Technologies Used

* Python 3
* GitHub REST API
* Requests
* JSON
* Google Colab
* GitHub

## Project Structure

```text
github-profile-analyzer/
│
├── GitHub_Profile_Analyzer.ipynb
├── README.md
└── requirements.txt
```

## Requirements

The project requires the `requests` library.

### requirements.txt

```text
requests
```

## API Endpoints

### GitHub Profile API

```python
url = f"https://api.github.com/users/{username}"
```

### GitHub Repository API

```python
url = f"https://api.github.com/users/{username}/repos"
```

## Main Code

### Import Library

```python
import requests
```

### Get GitHub Profile

```python
def get_github_profile(username):
    url = f"https://api.github.com/users/{username}"

    try:
        response = requests.get(url, timeout=10)

        if response.status_code == 200:
            return response.json()

        elif response.status_code == 404:
            print("GitHub user not found.")
            return None

        elif response.status_code == 403:
            print("GitHub API rate limit reached.")
            return None

        else:
            print("Error:", response.status_code)
            return None

    except requests.exceptions.RequestException as error:
        print("Network error:", error)
        return None
```

### Get Repositories

```python
def get_repositories(username):
    url = f"https://api.github.com/users/{username}/repos"

    params = {
        "per_page": 100,
        "sort": "updated"
    }

    try:
        response = requests.get(
            url,
            params=params,
            timeout=10
        )

        if response.status_code == 200:
            return response.json()

        print("Unable to fetch repositories.")
        return []

    except requests.exceptions.RequestException as error:
        print("Network error:", error)
        return []
```

### Classify Developer

```python
def classify_profile(public_repos):

    if public_repos >= 20:
        return "Highly Active Developer"

    elif public_repos >= 10:
        return "Active Developer"

    elif public_repos >= 5:
        return "Growing Developer"

    else:
        return "Beginner Developer"
```

### Analyze Repositories

```python
def analyze_repositories(repositories):

    repo_names = []
    languages = []

    total_stars = 0
    total_forks = 0

    for repo in repositories:

        repo_names.append(
            repo.get("name", "Unknown")
        )

        language = repo.get("language")

        if language:
            languages.append(language)

        total_stars += repo.get(
            "stargazers_count", 0
        )

        total_forks += repo.get(
            "forks_count", 0
        )

    unique_languages = set(languages)

    language_count = {}

    for language in languages:

        if language in language_count:
            language_count[language] += 1
        else:
            language_count[language] = 1

    most_used_language = None

    if language_count:
        most_used_language = max(
            language_count,
            key=language_count.get
        )

    average_stars = 0

    if repositories:
        average_stars = (
            total_stars / len(repositories)
        )

    return {
        "repo_names": repo_names,
        "total_repos": len(repositories),
        "total_stars": total_stars,
        "total_forks": total_forks,
        "average_stars": average_stars,
        "unique_languages": unique_languages,
        "language_count": language_count,
        "most_used_language": most_used_language
    }
```

### Find Most-Starred Repository

```python
def find_most_starred_repository(repositories):

    if not repositories:
        return None

    most_starred = repositories[0]

    for repo in repositories:

        if repo.get(
            "stargazers_count", 0
        ) > most_starred.get(
            "stargazers_count", 0
        ):

            most_starred = repo

    return most_starred
```

### Search Repository

```python
def search_repository(repositories, name):

    for repo in repositories:

        if repo.get(
            "name", ""
        ).lower() == name.lower():

            return repo

    return None
```

### Search by Language

```python
def search_by_language(repositories, language):

    results = []

    for repo in repositories:

        repo_language = repo.get("language")

        if (
            repo_language
            and repo_language.lower() == language.lower()
        ):
            results.append(
                repo.get("name")
            )

    return results
```

## How the Project Works

```text
User enters GitHub username
          ↓
Python sends API request
          ↓
GitHub REST API
          ↓
JSON response
          ↓
Python processes the data
          ↓
Repository analysis
          ↓
Developer classification
          ↓
Profile report displayed
```

## Developer Classification

| Public Repositories | Level                   |
| ------------------- | ----------------------- |
| 20 or more          | Highly Active Developer |
| 10–19               | Active Developer        |
| 5–9                 | Growing Developer       |
| Less than 5         | Beginner Developer      |

## Extra Options

After analyzing the profile, the user can select:

```text
1. Search Repository
2. Search by Language
3. Exit
```

## How to Run

1. Open `GitHub_Profile_Analyzer.ipynb` in Google Colab.
2. Install the required library:

```python
!pip install requests
```

3. Run the program.
4. Enter a GitHub username.
5. View the profile and repository analysis.
6. Use the extra search options.

## Example

```text
============================================================
           GITHUB PROFILE ANALYZER
============================================================

Enter GitHub username: octocat

Fetching GitHub profile...
Fetching repositories...

==================================================
              GITHUB PROFILE
==================================================
Name         : The Octocat
Username     : octocat
Bio          : ...
Location     : ...
Public Repos : 8
Followers    : ...
Following    : ...

Developer Level: Growing Developer
```

## Conclusion

GitHub Profile Analyzer is a simple Python application that demonstrates how to work with a real-world REST API. It retrieves GitHub data in JSON format and uses Python functions, loops, conditions, lists, dictionaries, and sets to analyze developer activity and repositories.
