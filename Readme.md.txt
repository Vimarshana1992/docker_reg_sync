Repository Sync Script - README
This document provides instructions and details for using the Repository Sync Script, which automates the synchronization of container images between two Docker registries.

Overview
The script:

Verifies the existence of all repositories listed in a file (reponames) in the source registry.
Fetches tags for each repository.
Identifies new tags that are not already processed.
Pulls, retags, and pushes these new tags to the target registry.
Tracks processed tags to avoid redundant work.
Prerequisites
1. Dependencies
Ensure the following tools are installed and accessible on the system:

jq: For parsing JSON responses from the Docker registry API.
docker: For managing images and interacting with Docker registries.
2. Configuration
Update the configuration section in the script to reflect your environment:

SOURCE_REPO: Source registry address.
TARGET_REPO: Target registry address.
REPO_NAMES_FILE: Path to the file containing repository names.
TAG_TRACK_DIR: Path to the directory for tracking processed tags.
3. Input File
Create a text file (e.g., /home/ubuntu/docreg/reponames) with the names of the repositories you want to sync. Each repository name should be on a new line:

plaintext
Copy code
hello-world
nginx
alpine
Usage
1. Clone or Copy the Script
Save the script to a file, e.g., sync_repos.sh, and make it executable:

bash
Copy code
chmod +x sync_repos.sh
2. Run the Script
Execute the script:

bash
Copy code
./sync_repos.sh
The script performs the following actions:

Validates that all repositories listed in reponames exist in the source registry.
Fetches tags for each repository.
Synchronizes new tags to the target registry.
Logging
The script logs its actions with timestamps. Example log entries:

plaintext
Copy code
2024-12-09 22:15:00 - Verifying existence of all repositories in the source registry.
2024-12-09 22:15:01 - All repositories exist in the source registry. Proceeding with sync.
2024-12-09 22:15:02 - Processing repository: nginx
2024-12-09 22:15:03 - Pulling nginx:latest from 192.168.48.129:5000
2024-12-09 22:15:05 - Tagging nginx:latest as 192.168.48.129:6000/nginx:latest
2024-12-09 22:15:06 - Pushing nginx:latest to 192.168.48.129:6000
If a repository does not exist:

plaintext
Copy code
2024-12-09 22:15:01 - Error: Repository 'nginx' does not exist in the source registry.
2024-12-09 22:15:01 - Exiting script due to missing repository: nginx.
Error Handling
Repository Not Found: The script exits immediately if any repository listed in reponames does not exist in the source registry. Log example:

plaintext
Copy code
2024-12-09 22:15:01 - Error: Repository 'nginx' does not exist in the source registry.
Failed Tag Fetch: If the script cannot fetch tags for a repository, it logs the error and exits.

Directory Structure
/home/ubuntu/docreg/reponames: File containing the list of repositories.
/home/ubuntu/docreg/tags: Directory to track processed tags for each repository.
Future Enhancements
Add support for ignoring certain repositories with a flag or configuration.
Include retries for failed Docker operations.
Add email or Slack notifications for sync results.
License
This script is open-source and provided as-is without warranty. Modify and use it according to your needs.

Support
For questions or issues, feel free to reach out or open a discussion on your preferred platform.






