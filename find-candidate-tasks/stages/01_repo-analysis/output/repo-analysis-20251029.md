# Repo Analysis: NearBeach (https://github.com/nearbeach/NearBeach)

## Repository Overview
- **Language/Framework**: Python (Django) + JavaScript (Vue.js)
- **Key Directories**:
  - `NearBeach/` - Main Django app
  - `NearBeach/views/` - View controllers (task_views.py, project_views.py, search_views.py, note_views.py, etc.)
  - `NearBeach/models.py` - 2000+ line models file
  - `NearBeach/forms.py` - Form definitions
  - `NearBeach/tests/` - Django test suite organized by feature
  - `src/` - Vue.js frontend
- **How to Run** (from repo-profiles.md):
  ```bash
  cd /workspace/nearbeach
  python3 -m pip install --user --break-system-packages -r requirements.txt
  python3 manage.py migrate
  DJANGO_SUPERUSER_USERNAME=admin DJANGO_SUPERUSER_EMAIL=admin@example.com DJANGO_SUPERUSER_PASSWORD=password python3 manage.py createsuperuser --noinput
  python3 manage.py runserver 0.0.0.0:8000
  ```
  Then open `http://localhost:8000` and log in as `admin` / `password`.

## Good Task Areas (from profile)
- Task and project views, lists, filtering and sorting
- Forms and validation
- Permissions
- Notes and attachments
- Search
- Django test suite

## Candidate Tasks

### Candidate 1: Task List View with Filtering, Sorting, and Pagination
- **Area**: Task and project views, lists, filtering and sorting
- **Specific Problem**: NearBeach has `task_information` (detail view) and `new_task` (create view), but **no general task list view** for users to see all their tasks with filtering, sorting, and pagination. Users must navigate through projects or kanban boards to find tasks. A dedicated task list is a standard project management feature.
- **Why It Matters**: Project managers and team members need a central place to view, filter, and sort all tasks across projects. Without this, finding tasks requires clicking through multiple projects or using search (which doesn't have task-specific filters).
- **Preliminary Completion Criteria**:
  - New URL route `/tasks/` showing paginated task list
  - Filter by: task status, assignee, project, date range (start/end), priority, story points
  - Sort by: creation date, start date, end date, priority, status, project
  - Page size selector (10, 25, 50, 100)
  - Respects existing permission system (user sees only tasks they have access to)
  - Unit tests for filtering, sorting, pagination, and permissions
- **Existing Solution Check**:
  - GitHub issues: No open issues for "task list" (repo shows 0 open issues on GitHub)
  - GitHub PRs: No recent PRs adding task list view
  - Recent commits: No commits in last 6 months touching task list functionality
  - Code search: No `task_list` view in `views/task_views.py` or `urls.py`

---

### Candidate 2: Unified Search with Faceted Filtering
- **Area**: Search
- **Specific Problem**: Search is fragmented across **7 separate endpoints** (`search`, `search_data`, `search_customer`, `search_customer_data`, `search_organisation`, `search_organisation_data`, `search_group`, `search_group_data`, `search_permission_set`, `search_permission_set_data`, `search_sprint`, `search_sprint_data`, `search_user`, `search_user_data`, `search_notification`, `search_notification_data`, `search_tag`, `search_scheduled_objects`) with **duplicated filtering logic** in each. No faceted filtering (filter by object type, date range, status after initial search).
- **Why It Matters**: Users cannot refine search results by type, date, or status after searching. The fragmented codebase makes maintenance hard and inconsistent behavior likely.
- **Preliminary Completion Criteria**:
  - Single unified search endpoint supporting all object types
  - Faceted sidebar: filter by object type (task, project, requirement, RFC, sprint, customer, organisation, user), status (open/closed), date range
  - Facet counts update dynamically
  - Maintains existing permission checks per object type
  - Backward compatible with existing search URLs
  - Unit tests for each facet filter and combination
- **Existing Solution Check**:
  - GitHub issues: No open issues for unified/faceted search
  - GitHub PRs: No recent PRs consolidating search endpoints
  - Recent commits: Search views largely unchanged in last 6 months
  - Code search: Confirmed 18+ search functions across `search_views.py` with duplicated `for split_row in search.split(" ")` pattern

---

### Candidate 3: Notes Enhancement - Rich Text, Mentions, and Threading
- **Area**: Notes and attachments
- **Specific Problem**: Notes (`ObjectNote` model, `note_views.py`) are **plain text only** - no rich text formatting, no @mentions to notify users, no threading/replies. The `add_notes` and `note_list` views only handle basic CRUD.
- **Why It Matters**: Team collaboration on tasks/projects requires richer notes: formatting for clarity, @mentions to pull in stakeholders, and threaded discussions. Current notes are too limited for real collaboration.
- **Preliminary Completion Criteria**:
  - Rich text support (TinyMCE already used elsewhere in codebase - integrate it)
  - @mention autocomplete (search users, insert mention, send notification)
  - Threaded replies (parent_note FK on ObjectNote, UI for expand/collapse threads)
  - Notification integration (reuse existing Notification model)
  - Backward compatible with existing plain-text notes
  - Unit tests for mention parsing, notification creation, thread rendering
- **Existing Solution Check**:
  - GitHub issues: No open issues for rich text notes or mentions
  - GitHub PRs: No recent PRs enhancing notes beyond basic CRUD
  - Recent commits: `note_views.py` largely unchanged; TinyMCE used in task/project forms but not notes
  - Code search: `ObjectNote` model has only `object_note` TextField - no parent_note, no rich text flag

---

### Candidate 4: Search Test Coverage - Data-Driven Unit Tests
- **Area**: Django test suite
- **Specific Problem**: `tests/tests_views/tests_search/test_search.py` has **explicit TODOs**: "Add in the UNIT TESTS for testing data in produces certain data out" for customer search, organisation search, and main search. Current tests only check HTTP 200 status, not actual search result correctness.
- **Why It Matters**: Search is a critical feature with complex multi-object logic. Without data-driven tests, regressions in filtering, pagination, or permissions go undetected.
- **Preliminary Completion Criteria**:
  - Create test fixtures with known data (tasks, projects, customers, organisations with specific names/dates)
  - Tests for each search type: main search, customer, organisation, group, user, sprint, tag, notification, permission set
  - Verify: exact match, partial match, multi-term AND logic, case insensitivity, pagination boundaries, permission filtering
  - Target: 80%+ coverage on `search_views.py` and `search/search_objects.py`
  - All new tests pass in CI
- **Existing Solution Check**:
  - GitHub issues: No open issues for search test coverage
  - GitHub PRs: No recent PRs adding data-driven search tests
  - Recent commits: Test file unchanged since initial creation (TODOs still present)
  - Code search: Only 2 test classes, ~100 lines covering happy-path status codes only

---

### Candidate 5: Form Validation Enhancement - Cross-Field and Business Rule Validation
- **Area**: Forms and validation
- **Specific Problem**: Forms (`NewTaskForm`, `TaskInformationForm`, `NewProjectForm`, `ProjectForm` in `forms.py`) have **only field-level validation**. Missing: cross-field validation (end_date >= start_date), business rule validation (story points only for certain statuses), and user-friendly error messages.
- **Why It Matters**: Users can submit invalid data (end date before start date, story points on closed tasks) leading to confusing UX or data integrity issues. Validation should catch this at form level with clear messages.
- **Preliminary Completion Criteria**:
  - Cross-field validation: `task_end_date >= task_start_date`, `project_end_date >= project_start_date`
  - Business rule: story points only editable when task status is not "Closed"
  - Business rule: priority required when creating task in active project
  - Clear, field-specific error messages returned via JSON for AJAX forms
  - Unit tests for each validation rule (valid/invalid cases)
  - Integration tests for form submission via views
- **Existing Solution Check**:
  - GitHub issues: No open issues for form validation
  - GitHub PRs: No recent PRs adding cross-field validation
  - Recent commits: `forms.py` has only basic field definitions, no `clean()` methods
  - Code search: Forms inherit from `forms.ModelForm` with no custom `clean` methods

---

## Notes
- **Codebase maturity**: Well-structured Django app with clear separation of views, models, forms. Permission system is group-based and consistently applied via decorators.
- **Promising areas**: Task list (missing entirely), search (fragmented but functional), notes (basic but extensible with existing TinyMCE), tests (explicit gaps documented).
- **Areas to avoid**: Live collaboration (not in repo), OAuth/SSO (blocked by external deps), API (blocked by OAuth), document purge script (ops-focused).
- **Test setup**: Uses `NearBeach_basic_setup.json` fixture. Run with `python3 manage.py test NearBeach.tests.tests_views.tests_search`.