# user_check


To achieve these objectives for determining user proficiency through project contributions (e.g., code pushes, pull requests, merges, rejections), you can build a comprehensive system that tracks and evaluates each user's activity. Here’s a detailed explanation on how to implement each of the outlined points:

# Code Contributions (Pushes)
How to Implement:

Data Collection: Use the Git API (e.g., GitHub, GitLab, Bitbucket) to fetch the number of commits a user has pushed. Each platform provides APIs to track commits by user, date, and repository.
Code Language Detection:
Each commit contains a diff of the code changes, from which you can analyze the file extensions to detect programming languages.
Use libraries or tools (like GitHub Linguist or custom language detection scripts) to identify the percentage of code in various languages.
Weighting: Assign higher proficiency scores based on the frequency of commits in specific languages. For instance, if 50% of the user’s contributions are in Python, their proficiency in Python is rated higher.

Tools to Use:

GitHub API (for fetching user-specific commits): /repos/{owner}/{repo}/commits
GitLab API, Bitbucket API for similar actions.

# Example: Fetch user commits from GitHub API
import requests

response = requests.get('https://api.github.com/repos/{owner}/{repo}/commits?author={user}')
commits = response.json()
for commit in commits:
    print(commit['commit']['message'])


2. Pull Request Creation
How to Implement:

Data Collection: Use the API endpoints provided by Git platforms to track the number of pull requests (PRs) created by each user.
Weighting: Higher weight can be given to PRs tied to feature development, compared to small bug fixes. This can be determined by looking at commit messages or branch names (e.g., feature/*).
PR Type Categorization: Use metadata or labels in your version control system to categorize PRs by type (feature, bug fix, refactoring, etc.).
Tools to Use:

GitHub API: /repos/{owner}/{repo}/pulls
GitLab API for merge requests.
Bitbucket API.
Example:

python
# Example: Fetch pull requests created by a user
response = requests.get('https://api.github.com/repos/{owner}/{repo}/pulls?state=all&creator={user}')
prs = response.json()
for pr in prs:
    print(f"PR Title: {pr['title']}, State: {pr['state']}")


# Example: Fetch pull requests created by a user
response = requests.get('https://api.github.com/repos/{owner}/{repo}/pulls?state=all&creator={user}')
prs = response.json()
for pr in prs:
    print(f"PR Title: {pr['title']}, State: {pr['state']}")
3. Pull Request Merges
How to Implement:

Data Collection: Track the PRs that were successfully merged using the API. Many platforms provide this information directly, or you can look at PR statuses (merged, closed, rejected).
Weighting: Merged PRs indicate successful contributions. You can assign higher weight to these PRs since they passed code reviews and met team standards.
Track Reviews: Include how often a user’s code passes on the first review (i.e., without significant changes requested).
Tools to Use:

GitHub API: /repos/{owner}/{repo}/pulls with the merged state.
GitLab API, Bitbucket API for similar actions.
Example:

python
Copy code
# Example: Fetch merged pull requests
response = requests.get('https://api.github.com/repos/{owner}/{repo}/pulls?state=closed')
merged_prs = [pr for pr in response.json() if pr['merged_at'] is not None]
for pr in merged_prs:
    print(f"PR: {pr['title']} merged at {pr['merged_at']}")
4. Pull Request Rejections
How to Implement:

Data Collection: Track the PRs that were closed without being merged or marked as rejected. This can be directly retrieved from Git APIs.
Weighting: If a PR is rejected or closed frequently, it might indicate lower proficiency, especially if the reason is poor code quality or failure to meet requirements. Weight users' proficiency lower based on frequent PR rejections.
Examine Feedback: If possible, analyze the code review feedback or rejection reasons to determine why a PR was rejected.
Tools to Use:

GitHub API: /repos/{owner}/{repo}/pulls?state=closed
Use a filter to check if the PR was merged or not.
Example:

python
Copy code
# Example: Fetch rejected (closed but not merged) pull requests
prs = requests.get('https://api.github.com/repos/{owner}/{repo}/pulls?state=closed').json()
rejected_prs = [pr for pr in prs if pr['merged_at'] is None]
5. Type of Contribution
How to Implement:

Data Collection: Categorize PRs or commits based on whether they are for new features, bug fixes, refactoring, or documentation.
Weighting: New feature development or large refactoring projects are generally more complex and deserve higher weight in proficiency evaluation.
Methodology:
Branch Naming Conventions: Use naming conventions like feature/* for features, bugfix/* for bug fixes to categorize contributions.
Commit Message Parsing: Use keywords in commit messages to identify contribution types (e.g., “add”, “fix”, “refactor”).
Tools to Use:

GitHub, GitLab, or Bitbucket APIs to fetch commit and branch data.
Natural Language Processing (NLP) for commit message analysis.
6. Code Review Participation
How to Implement:

Data Collection: Track how often a user participates in reviewing others’ code. This can be done by fetching the number of reviews a user has submitted.
Weighting: Users who provide high-quality reviews, especially those that are accepted by the code author, show proficiency in the skill.
Automating: Pull data for the number of PRs a user has reviewed, along with the depth of feedback.
Tools to Use:

GitHub API: /repos/{owner}/{repo}/pulls/reviews
GitLab and Bitbucket APIs have similar endpoints to fetch code reviews.
Example:

python
Copy code
# Example: Fetch reviews by a user
response = requests.get('https://api.github.com/repos/{owner}/{repo}/pulls/{pull_number}/reviews')
reviews = response.json()
for review in reviews:
    print(f"Review by: {review['user']['login']}, State: {review['state']}")
7. Language Breakdown
How to Implement:

Data Collection: Analyze the types of files a user contributes to (e.g., .py for Python, .js for JavaScript).
Language Detection: Use tools like GitHub’s Linguist library or manual file extension mapping to determine the programming languages.
Weighting: Users proficient in certain languages will have higher contributions in those languages. Use this to assign weight to a user's proficiency in each language.
Tools to Use:

Linguist or similar libraries to detect languages based on file extensions.
8. Issue Resolution and Commit Linkage
How to Implement:

Data Collection: Track the issues that a user resolves by linking commits or PRs to issues. You can link Jira, GitHub Issues, or GitLab Issues with commits or PRs.
Weighting: Solving more complex or high-priority issues should increase the user’s skill rating.
Tools to Use:

Jira API for issue-tracking.
GitHub API for linked issues: /repos/{owner}/{repo}/issues.
9. Task Complexity
How to Implement:

Data Collection: Assign complexity scores to tasks (e.g., via Jira or other project management tools).
Weighting: More complex tasks should carry higher weight when assessing user proficiency.
Task Linkage: Track which users are assigned and complete these tasks, linking them to their commits or PRs.
Tools to Use:

Jira API or any project management tool that tracks task complexity and assignment.
Automation Tools and Infrastructure
CI/CD Integration: Use CI/CD pipelines (like Jenkins, CircleCI) to automatically track commits, PRs, and other metrics in real-time.
Visualization: Build dashboards (e.g., using Grafana, Kibana, or custom web apps) to visualize contributions and skill levels.
Storage: Store the analyzed data in a database (e.g., PostgreSQL, MongoDB) for later reporting or evaluation.
By combining these steps into an automated workflow, you can create a comprehensive system for objectively evaluating user proficiency based on their real contributions.








