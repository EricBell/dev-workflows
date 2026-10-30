# Selected Task for LiveCompare

## User Selection
- **Chosen**: Task List View with Filtering, Sorting, and Pagination
- **Score**: 25/25
- **Selection Date**: 2025-10-29
- **User Notes**: No modifications - use Task Brief as-is

## Complete Task Brief (Ready for LiveCompare)

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

## Next Steps for User
1. Copy the Task Brief above into LiveCompare (Task Description, How to Judge Completion, Other Notes for Reviewer)
2. Download the NearBeach repo zip from the approved repos table in project-rules.md
3. Mount the archive in LiveCompare before your first message
4. Use the File Source URL: `https://github.com/nearbeach/NearBeach`
5. Start the conversation with the model using the Task Description as your first message