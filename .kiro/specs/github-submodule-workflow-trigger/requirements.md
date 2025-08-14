# Requirements Document

## Introduction

This feature enables automatic triggering of Playbook-Door repository (parent) workflows when PlayBook repository (child) is updated in GitHub. The system uses GitHub repository dispatch events to establish communication between the child repository and parent repository, allowing for automated synchronization and deployment when new projects are added or modified in the PlayBook repository.

## Requirements

### Requirement 1

**User Story:** As a PlayBook repository maintainer, I want Playbook-Door repository workflows to automatically trigger when PlayBook repository is updated, so that the website automatically reflects new projects and changes.

#### Acceptance Criteria

1. WHEN PlayBook repository receives a push to main branch THEN it SHALL trigger a GitHub Actions workflow
2. WHEN PlayBook workflow runs THEN it SHALL send a repository dispatch event to Playbook-Door repository
3. WHEN Playbook-Door receives the dispatch event THEN it SHALL trigger its deployment workflow
4. IF the dispatch event fails to send THEN the PlayBook workflow SHALL log the failure with clear error messages

### Requirement 2

**User Story:** As a developer, I want to configure which file changes trigger the parent workflow, so that I can control when synchronization occurs and avoid unnecessary builds.

#### Acceptance Criteria

1. WHEN configuring the PlayBook workflow THEN it SHALL allow specification of file paths that trigger the workflow
2. WHEN only __meta__.txt files change THEN the workflow SHALL trigger the parent repository update
3. WHEN new project directories are added THEN the workflow SHALL trigger the parent repository update
4. IF changes occur outside monitored paths THEN the workflow SHALL NOT trigger unnecessary parent updates

### Requirement 3

**User Story:** As a website maintainer, I want Playbook-Door to automatically update its submodule and regenerate the project directory listing, so that the website stays current with PlayBook repository changes.

#### Acceptance Criteria

1. WHEN Playbook-Door workflow is triggered THEN it SHALL update the PlayBook submodule to the latest commit
2. WHEN submodule is updated THEN the system SHALL scan project directories for __meta__.txt files
3. WHEN scanning is complete THEN the system SHALL generate an updated directories.json file
4. WHEN directories.json is updated THEN the system SHALL deploy the changes to GitHub Pages

### Requirement 4

**User Story:** As a system integrator, I want to use GitHub API repository dispatch events to communicate between repositories, so that I have a reliable method for cross-repository workflow triggering.

#### Acceptance Criteria

1. WHEN PlayBook workflow runs THEN it SHALL send a POST request to GitHub API dispatches endpoint
2. WHEN sending dispatch events THEN the system SHALL include event_type "submodule-update" and relevant metadata
3. WHEN dispatch event includes client_payload THEN it SHALL contain repository name, ref, and timestamp information
4. WHEN API request is made THEN it SHALL use proper authentication headers with GitHub token

### Requirement 5

**User Story:** As a system administrator, I want proper error handling and logging for cross-repository workflow triggers, so that I can troubleshoot issues when synchronization fails.

#### Acceptance Criteria

1. WHEN PlayBook workflow completes successfully THEN it SHALL log "已通知父仓库更新" message
2. WHEN API request fails THEN the system SHALL log detailed error information including HTTP status codes
3. WHEN authentication fails THEN the system SHALL provide clear error messages about token permissions
4. WHEN Playbook-Door workflow runs THEN it SHALL log submodule update status and scanning results

### Requirement 6

**User Story:** As a security-conscious developer, I want secure authentication between repositories, so that cross-repository access is properly controlled and audited.

#### Acceptance Criteria

1. WHEN accessing Playbook-Door repository THEN the system SHALL use GitHub Personal Access Token
2. WHEN configuring authentication THEN the token SHALL be stored as PARENT_REPO_TOKEN repository secret
3. WHEN creating the token THEN it SHALL have "repo" and "workflow" scopes for full repository access
4. IF the token lacks required permissions THEN the API request SHALL fail with 403 Forbidden error

### Requirement 7

**User Story:** As a developer, I want to include additional commit information in the dispatch payload, so that the parent repository has context about what changes triggered the update.

#### Acceptance Criteria

1. WHEN sending dispatch events THEN the system SHALL include the latest commit message in client_payload
2. WHEN sending dispatch events THEN the system SHALL include the commit SHA in client_payload
3. WHEN Playbook-Door receives the dispatch THEN it SHALL have access to commit metadata for logging
4. WHEN multiple commits are pushed together THEN the system SHALL use the latest commit information