# Task Briefs for LiveCompare

## Candidate 1: Task List View with Filtering, Sorting, and Pagination
**Score**: 25/25 | **Estimated Time**: 4-6 hours | **Complexity**: Medium-High - new view, template, filter/sort/pagination logic, permissions, 10+ tests

### Task Description
NearBeach has task detail views (`task_information`) and task creation (`new_task`), but **no general task list view** for users to see all their tasks across projects. Users must navigate through individual projects or kanban boards to find tasks, which is inefficient for project managers overseeing multiple projects. A dedicated task list with filtering, sorting, and pagination is a standard project management feature that would significantly improve task discoverability and workflow.

### How to Judge Completion
In the repo folder, run the NearBeach start commands:
```bash
cd /workspace/nearbeach
python3 -m pip install --user --break-system-packages -r requirements.txt
python3 manage.py migrate
DJANGO_SUPERUSER_USERNAME=admin DJANGO_SUPERUSER_EMAIL=admin@example.com DJANGO_SUPERUSER_PASSWORD=password python3 manage.py createsuperuser --noinput
python3 manage.py runserver 0.0.0.0:8000
```
Then open `http://localhost:8000` and log in as `admin` / `password`.

**Verification steps:**
1. Navigate to `/tasks/` (new URL) - should display a paginated table of all tasks the user has permission to view
2. Test filtering: select a status from dropdown → table updates to show only tasks with that status
3. Test filtering: select an assignee → shows only tasks assigned to that user
4. Test filtering: select a project → shows only tasks in that project
5. Test filtering: enter date range for start/end dates → shows only tasks in range
6. Test sorting: click column headers (Created, Start Date, End Date, Priority, Status, Project) → toggles asc/desc
7. Test pagination: page size selector (10, 25, 50, 100) and page navigation work
8. Run `python3 manage.py test NearBeach.tests.tests_views.tests_task` - should pass with 10+ new tests covering filtering, sorting, pagination, and permissions
9. Verify permission enforcement: create a second user without access to a project, log in as that user, verify they cannot see tasks from that project

### Other Notes for Reviewer
NearBeach is an open-source Django/Vue project management tool for tracking requirements, projects, tasks, kanban boards, sprints, and RFCs. It's used by small teams to enterprises. The missing task list view is a significant UX gap - users currently have no central place to view all their tasks across projects. This task adds a core feature that project managers would expect, improving daily workflow for anyone managing multiple projects. The implementation touches Django views, templates, URL routing, the existing permission system (group-based via ObjectAssignment), and requires comprehensive tests to ensure permission filtering works correctly.

### Practical Details
- **Setup Commands**:
  ```bash
  cd /workspace/nearbeach
  python3 -m pip install --user --break-system-packages -r requirements.txt
  python3 manage.py migrate
  DJANGO_SUPERUSER_USERNAME=admin DJANGO_SUPERUSER_EMAIL=admin@example.com DJANGO_SUPERUSER_PASSWORD=password python3 manage.py createsuperuser --noinput
  python3 manage.py runserver 0.0.0.0:8000
  ```
- **Key Files Likely Involved**:
  - `NearBeach/views/task_views.py` - new `task_list` view function
  - `NearBeach/urls.py` - new route for `/tasks/`
  - `NearBeach/templates/NearBeach/tasks/task_list.html` - new template with filter sidebar, table, pagination
  - `NearBeach/forms.py` - new `TaskFilterForm` for filter validation
  - `NearBeach/tests/tests_views/tests_task/` - new test files
- **Potential Challenges**:
  - Permission filtering must respect existing `ObjectAssignment` group-based system
  - Filter combinations (status + assignee + project + date range) need efficient queryset construction
  - Vue frontend may need updates if task list integrates with existing UI patterns

---

## Candidate 2: Unified Search with Faceted Filtering
**Score**: 22/25 | **Estimated Time**: 5-7 hours | **Complexity**: High - refactoring 18+ search functions, facet logic, backward compatibility, permissions

### Task Description
NearBeach's search functionality is fragmented across **18+ separate view functions** in `search_views.py` (e.g., `search`, `search_data`, `search_customer`, `search_customer_data`, `search_organisation`, `search_organisation_data`, `search_group`, `search_permission_set`, `search_sprint`, `search_user`, `search_notification`, `search_tag`, `search_scheduled_objects`), each with **duplicated filtering logic** (splitting search terms by space and applying `icontains` filters). Users cannot refine results after searching - no faceted filtering by object type, status, or date range. This makes search rigid and the codebase hard to maintain.

### How to Judge Completion
In the repo folder, run the NearBeach start commands (same as Candidate 1). Then open `http://localhost:8000` and log in as `admin` / `password`.

**Verification steps:**
1. Navigate to `/search/` - should display unified search interface with single search box
2. Enter a search term (e.g., "project") → results show all matching objects across types (tasks, projects, requirements, RFCs, sprints, customers, organisations, users)
3. **Faceted sidebar** displays: Object Type (with counts), Status (Open/Closed with counts), Date Range (presets + custom)
4. Click an Object Type facet (e.g., "Task (12)") → results filter to only tasks, facet counts update
5. Click Status facet "Open" → results filter to open items only
6. Select date range → results filter to items created/modified in range
7. Combine multiple facets → results reflect all selected filters
8. Clear facets → returns to full results
9. **Backward compatibility**: old URLs (`/search_customer/`, `/search_organisation/`, etc.) still work and redirect or render appropriately
10. Run `python3 manage.py test NearBeach.tests.tests_views.tests_search` - should pass with 15+ new tests covering each facet, combinations, permissions, and pagination
11. Verify permission enforcement: results only show objects user has access to via `ObjectAssignment`

### Other Notes for Reviewer
NearBeach is an open-source Django/Vue project management tool. Search is a critical navigation feature but currently fragmented and limited. The 18+ search functions share ~80% duplicated logic, making bug fixes and improvements error-prone. Users cannot drill down into results (e.g., "show me only open tasks from last week matching 'bug'"). This task consolidates search into a single unified endpoint with faceted filtering - a significant UX and maintainability improvement. The challenge is preserving backward compatibility while refactoring, and ensuring the complex permission system (group-based via ObjectAssignment) is correctly applied across all object types in the unified results.

### Practical Details
- **Setup Commands**: (same as Candidate 1)
- **Key Files Likely Involved**:
  - `NearBeach/views/search_views.py` - major refactor: consolidate into unified `search_unified` view
  - `NearBeach/views/search/search_objects.py` - extend `SearchObjects` class for faceted logic
  - `NearBeach/urls.py` - new unified route, preserve old routes for compatibility
  - `NearBeach/templates/NearBeach/search/search_unified.html` - new template with facet sidebar
  - `NearBeach/forms.py` - new `UnifiedSearchForm` with facet fields
  - `NearBeach/tests/tests_views/tests_search/` - expanded test coverage
- **Potential Challenges**:
  - Permission checks differ per object type (task vs project vs customer) - must apply correctly in unified query
  - Facet counts must be performant (avoid N+1 queries)
  - Backward compatibility for existing bookmarked search URLs
  - Vue frontend integration if search results feed into components

---

## Candidate 3: Notes Enhancement - Rich Text, Mentions, and Threading
**Score**: 21/25 | **Estimated Time**: 4-5 hours | **Complexity**: Medium - model migration, TinyMCE integration, mention parsing, notification hook, thread UI

### Task Description
NearBeach's notes (`ObjectNote` model, `note_views.py`) are **plain text only** - no rich text formatting, no @mentions to notify users, and no threading/replies. The `add_notes` and `note_list` views only handle basic CRUD. Team collaboration on tasks/projects requires richer notes: formatting for clarity (bold, lists, code blocks), @mentions to pull in stakeholders, and threaded discussions to keep conversations organized. Current notes are too limited for real collaboration.

### How to Judge Completion
In the repo folder, run the NearBeach start commands (same as Candidate 1). Then open `http://localhost:8000` and log in as `admin` / `password`.

**Verification steps:**
1. Open a task or project information page → navigate to Notes section
2. **Rich text**: Click "Add Note" → TinyMCE editor loads (same as task/project description fields) → apply bold, italic, bullet list, code block → save → note renders with formatting preserved
3. **Mentions**: In editor, type `@` → autocomplete dropdown shows users with access to this object → select user → mention inserted as `@username` → save → mentioned user receives a Notification (check notification bell/icon)
4. **Threading**: On an existing note, click "Reply" → inline editor appears → write reply → save → reply appears indented under parent note with "X replies" toggle
5. Click "X replies" → expands/collapses thread
6. Run `python3 manage.py test NearBeach.tests.tests_views.tests_note` - should pass with new tests for: rich text save/render, mention parsing and notification creation, thread parent/child relationships, permission checks on replies
7. **Backward compatibility**: existing plain-text notes still display correctly (no TinyMCE artifacts)

### Other Notes for Reviewer
NearBeach is an open-source Django/Vue project management tool used by teams for collaboration. Notes are attached to tasks, projects, requirements, RFCs, kanban cards, and organisations - they're a primary communication mechanism. The current plain-text notes are a significant collaboration limitation: no formatting for technical details, no way to alert teammates, no conversation threading. This task leverages existing TinyMCE integration (already used in task/project forms) and the Notification model to add rich text, @mentions, and threading with minimal new dependencies. The model needs a `parent_note` FK for threading and a `is_rich_text` flag; the mention parsing can reuse Django's notification system.

### Practical Details
- **Setup Commands**: (same as Candidate 1)
- **Key Files Likely Involved**:
  - `NearBeach/models.py` - add `parent_note` FK and `is_rich_text` BooleanField to `ObjectNote`
  - `NearBeach/views/note_views.py` - enhance `add_notes`, `note_list`, add `reply_note` view
  - `NearBeach/forms.py` - new `RichTextNoteForm` with TinyMCE widget
  - `NearBeach/templates/NearBeach/notes/note_list.html` - thread rendering, reply UI
  - `NearBeach/static/NearBeach/js/notes.js` - mention autocomplete, thread toggle (new or extend)
  - `NearBeach/tests/tests_views/tests_note/` - new tests for rich text, mentions, threads
  - Migration file for model changes
- **Potential Challenges**:
  - Mention parsing must handle @username in rich text HTML (not just plain text)
  - Notification creation must not spam (deduplicate mentions in same note)
  - Thread rendering performance with deep nesting
  - TinyMCE config must match existing instance for consistency

---

## Candidate 4: Search Test Coverage - Data-Driven Unit Tests
**Score**: 21/25 | **Estimated Time**: 2-3 hours | **Complexity**: Low-Medium - fixture creation, data-driven tests for 7 search types, coverage verification

### Task Description
The search test file `tests/tests_views/tests_search/test_search.py` has **explicit TODOs** stating "Add in the UNIT TESTS for testing data in produces certain data out" for customer search, organisation search, and main search. Current tests only verify HTTP 200 status codes, not actual search result correctness. Search is a critical feature with complex multi-object filtering logic, and without data-driven tests, regressions in filtering, pagination, or permissions go undetected.

### How to Judge Completion
In the repo folder, run the NearBeach start commands (same as Candidate 1).

**Verification steps:**
1. Run `python3 manage.py test NearBeach.tests.tests_views.tests_search -v2` - should show 20+ new test methods passing
2. Run coverage: `coverage run --source='NearBeach.views.search_views,NearBeach.views.search' manage.py test NearBeach.tests.tests_views.tests_search` then `coverage report` - should show 80%+ coverage on `search_views.py` and `search/search_objects.py`
3. New tests verify:
   - Exact match, partial match, multi-term AND logic (space-split), case insensitivity
   - Pagination boundaries (page 1, page 2, page beyond results)
   - Permission filtering (user sees only objects they have access to via ObjectAssignment)
   - Each search type: main, customer, organisation, group, user, sprint, tag, notification, permission set
   - Closed/include_closed toggle works per object type
   - Facet counts (if unified search from Candidate 2 is done first) or result counts per type
4. All existing tests still pass

### Other Notes for Reviewer
NearBeach is an open-source Django/Vue project management tool. Search spans 9+ object types with complex permission logic. The current test suite has significant gaps documented as TODOs in the code itself. This task is pure test engineering - no feature code changes - but it dramatically improves confidence in search correctness and enables safer refactoring (like Candidate 2). The fixtures should create known data with specific names/dates to enable deterministic assertions.

### Practical Details
- **Setup Commands**: (same as Candidate 1)
- **Key Files Likely Involved**:
  - `NearBeach/tests/tests_views/tests_search/test_search.py` - expand with data-driven test methods
  - `NearBeach/fixtures/` - new fixture file with known search test data (or extend existing)
  - `NearBeach/views/search/search_objects.py` - may need minor testability improvements
- **Potential Challenges**:
  - Creating comprehensive fixtures that cover all object types and permission scenarios
  - Test data isolation (use `TransactionTestCase` or careful fixture design)
  - Coverage tool configuration

---

## Candidate 5: Form Validation Enhancement - Cross-Field and Business Rule Validation
**Score**: 21/25 | **Estimated Time**: 2-3 hours | **Complexity**: Low-Medium - add clean() methods to 4 forms, error handling, 8-10 tests

### Task Description
NearBeach's forms (`NewTaskForm`, `TaskInformationForm`, `NewProjectForm`, `ProjectForm` in `forms.py`) have **only field-level validation**. Missing: cross-field validation (end_date >= start_date), business rule validation (story points only editable for non-closed tasks), and user-friendly error messages. Users can submit invalid data (end date before start date, story points on closed tasks) leading to confusing UX or data integrity issues. Validation should catch this at form level with clear, field-specific error messages returned via JSON for AJAX forms.

### How to Judge Completion
In the repo folder, run the NearBeach start commands (same as Candidate 1). Then open `http://localhost:8000` and log in as `admin` / `password`.

**Verification steps:**
1. Open "New Task" form → enter end_date before start_date → submit → should return field-specific error on end_date: "End date must be on or after start date"
2. Open "Edit Task" on a closed task → try to change story_points → submit → should return error: "Story points cannot be modified on closed tasks"
3. Open "New Project" → end_date before start_date → submit → error on end_date
4. Open "Edit Project" → invalid priority value → error on priority field
5. All errors returned as JSON with field names for AJAX form handling (check Network tab)
6. Run `python3 manage.py test NearBeach.tests.tests_forms` (new test file) - should pass with 8-10 tests covering valid/invalid cases for each rule
7. Verify existing form submissions still work (no regression)

### Other Notes for Reviewer
NearBeach is an open-source Django/Vue project management tool. Forms are the primary data entry point for tasks, projects, requirements, and RFCs. Currently, validation only happens at the field level (required, max_length, etc.). Business rules like "end date after start date" or "no story points on closed tasks" are enforced only at the database/model level (if at all), causing confusing 500 errors or silent data corruption. This task adds proper form-level `clean()` methods with clear, field-specific error messages - a straightforward but high-impact improvement to data integrity and UX. The forms already use AJAX submission, so JSON error responses are expected.

### Practical Details
- **Setup Commands**: (same as Candidate 1)
- **Key Files Likely Involved**:
  - `NearBeach/forms.py` - add `clean()` methods to `NewTaskForm`, `TaskInformationForm`, `NewProjectForm`, `ProjectForm`
  - `NearBeach/views/task_views.py` - ensure JSON error responses for `new_task_save`, `task_information_save`
  - `NearBeach/views/project_views.py` - ensure JSON error responses for `new_project_save`, `project_information_save`
  - `NearBeach/tests/tests_forms/` - new test file (or extend existing)
- **Potential Challenges**:
  - AJAX error handling must match existing frontend expectations
  - Business rules may need coordination with model-level constraints
  - Date/timezone handling in cross-field validation