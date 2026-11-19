# Changelog

All notable changes to `qalainau/filament-warp-capacity` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.1] - 2026-11-20

### Fixed

- **Console error `warpCapacityPlanner is not defined`**: on pages where Livewire re-renders the Capacity planner inside another Alpine component, the browser console could show `warpCapacityPlanner is not defined` before the component's script had arrived. Empty components are now registered up front, and the real ones replace them as soon as they load. No visible change.

## [1.0.0] - 2026-11-19

### Added

- `demand(false)` and `baselines(false)` on the plugin leave out the *Demand and supply* and *Baselines* pages (and their routes) for teams that don't need them. The dashboard widget then no longer links to *Demand and supply*.
- `navigationGroup()` and `settingsNavigationGroup()` take a closure too, run when the menu is drawn, so the headings can follow each user's language (`fn () => __('Resourcing')`).
- `canAccess()` on the plugin: who may open Warp Capacity at all. Everyone else sees none of its pages in the menu (useful when only some teams of a panel use it).
- Planner: people (or projects) down the side and days, weeks or months across, with a strip per person showing how full each column is. Drag bookings to move them in time or to another person, drag their ends, hold Alt to copy, drag across an empty row to book, undo and redo. Only the rows on screen are drawn; months load as you scroll.
- Units: percent of each person’s time, hours, FTE and person-months; book in % of their time, hours a day, hours a week or FTE.
- Working weeks per person, employment dates, holidays for everyone and personal time off (whole or half days), also from another source with `timeOffUsing()`.
- Placeholders for roles not filled yet, and *Find someone*, ranked by role, skills and free time.
- Tentative projects with a chance of winning: count fully, weigh by chance or leave out.
- Scenarios kept apart from the live plan, compared on *Demand and supply* and applied when ready, with changes made meanwhile in the live plan reported.
- Demand and supply by role, week or month: booked work, placeholders and tentative work against working time, with the periods a role is short in.
- Weekly timesheets: fill in from the plan, submit, take back, approve or send back with a reason, approve several at once. Actuals from another source with `actualsUsing()`.
- Projects: budgets in hours and money, spent so far, forecast at the end, revenue, cost and margin from bill and cost rates per role or person; project settings.
- `ProjectCapacity` schema component for a project’s page, and the `CapacityOverview` dashboard widget.
- Excel and CSV download of the plan.
- Saves refused (and the plan reloaded) when someone else changed the same booking or timesheet meanwhile.
- Permissions (`canPlan`, `canManage`, `canApproveTimesheets`), Filament tenancy, dark mode, phone width, keyboard access to bookings.
- Multi-tenancy: people and projects are limited to the current team through the panel’s ownership relationship (or `tenantRelationship:` on `people()` and `projects()`); people, projects, placeholders and bookings of another team sent from the browser are refused; each panel keeps its own plugin options; a panel without tenancy only shows rows saved without a tenant. The tenant is stored as `tenant_type` and `tenant_id`, so panels with different tenant models (`Team` and `Company`) never mix rows with the same ID.
- Rate changes: from a date, a role or a person gets new bill and cost rates. Hours before the date keep the earlier rate, bookings across the date are split, and the lists show today’s rate with the next change.
- Notifications (`->notifications()`): *your plan has changed* (one message per person once the changes settle), *your week* on Mondays, timesheet reminders and *sent back* with the reason; by mail or in the panel’s bell; with the commands `warp-capacity:send-plan-changes`, `warp-capacity:send-week-ahead` and `warp-capacity:remind-timesheets`.
- Booking requests (`->bookingRequests()`, `canApproveBookings()`): people who cannot approve make requests, shown as dashed bars; *Requests* approves them, books someone else or declines with a reason, and tells the person who asked.
- `Bookings`: book, change, delete, approve and decline from your own code, `Bookings::sync()` to keep one booking per record of yours (Warp Scheduler tasks, orders), `Bookings::forTenant()` outside the panel; and the events `BookingCreated`, `BookingUpdated`, `BookingDeleted`, `BookingRequested`, `BookingApproved` and `BookingDeclined`.
- Billable utilization: the *Utilization* page (planned and from timesheets, by person and month or week) against a target per person, role or `utilizationTarget()`, and the `BillableUtilizationOverview` widget.
- Baselines: keep a copy of the plan and compare it with the plan now (bookings added, changed and removed; hours by project and person).
- Hiring forecast on *Demand and supply*: roles short for four weeks in a row, how many people to hire and when to start (`hiringLeadTime()`).
- Phases with their own budgets and milestones on projects, shown on the project rows of the planner; bookings can say which phase they are for.
- Repeating bookings: only on some weekdays, every 1 to 4 weeks.
- Calendar feeds (`->calendarFeeds()`): each person’s bookings and time off as a private iCal link; `IcsTimeOff` reads time off from people’s own calendars.
- Suggestions for overbooked people: give a booking to a free colleague, move it later or cut it, in one click.
- Timesheets: closing up to a date, a timer that fills the timesheet, importing past hours.
- Billing: approved hours as invoice lines (Excel, CSV and `Billing::lines()`), marking them as invoiced; monthly fees with an hour range (settlement above and below the range); projects billed in another currency with exchange rates.
- Monthly work reports per project, to print or save as PDF (and `WorkReport::for()` for Warp Print templates).
- Importing bookings and past hours from CSV files.
- English, Japanese, German, French, Spanish, Italian, Brazilian Portuguese, Korean, simplified and traditional Chinese.
- The project templates also ask your app's policy (`viewAny`) when you have one for the model, together with `canManageTemplates()`. So do the roles and time off settings, together with `canManage()`.
- `canAccessUsing(fn ($user, string $page) => …)` and `canWriteUsing(…)` on the plugin let your app decide, page by page, who can open each Warp Capacity page and who can only look at it (write buttons hidden, write calls answered with 403). They add to `canAccess`, `canPlan`, `canManage` and the other rules and never widen them. See *Restricting pages per role* in the README.
- Planner: **Split** in the booking form cuts a booking in two on a day. Both parts keep the amount, note, phase and weekdays.
- Planner: **Shift+click** (or Ctrl/⌘+click) selects several bars. Drag one to move them all by the same number of days — and to another person when they are all on one row — or press **Delete** to delete them. If one of them cannot be changed, none is.
- Planner: in the Projects view, the two-arrows button on a project row shifts the whole project earlier or later by days or weeks, optionally only what starts on or after a day, with its phases and milestones (for people who manage the settings, in the live plan). Bookings that cannot be moved are skipped and counted in the message.
- Planner: **Ctrl+Z** / **Ctrl+Shift+Z** also undo and redo a split, a move or delete of selected bars and a project shift.
- `Bookings::split($booking, $on)` and `Bookings::shiftProject($project, days: 7)` do the same from code.
- **Project templates**: keep the phases, milestones, roles to book and budget of a typical project, counted in days from its start, and plan a new project by choosing a start date (*Projects* → ⋮ → **Plan with a template**). **Save as a template** makes one from a project you already planned; **Remove what a template added** takes one application away again and puts the budget back. Templates are managed under *Project templates* by people who may manage the settings.
- `Templates::create()`, `fromProject()`, `preview()`, `apply()`, `undo()` and `applications()` do the same from code.
- Planner: **Skills and tags** in the toolbar shows only the people who have every skill and tag you tick. People get **tags** (*People* → the person), free labels such as *Remote*.
- Planner: **Find who is free** lists the people with time left between two dates — optionally with enough free hours, a role, skills and tags — and shows only them in the planner, with their free hours. `Availability::find()` does the same from code.
- Planner: **Saved views** keep People/Projects, zoom, unit, tentative mode, search, role and the ticked skills and tags under a name; people who manage the settings can share a view with everyone.
- `templates(false)` turns project templates off for a panel and `canManageTemplates(fn)` says who may use them.
- **Read-only links** for clients: *Projects* → ⋮ → **Share a read-only link** makes a signed address with an unguessable key that shows one project’s phases, milestones and who works on it week by week, without signing in, and prints on A4. You choose how long it works and whether it shows names, hours and the budget (money is off by default; rates, cost, revenue and margin are never shown). **Shared links** lists and revokes them; a revoked or expired link answers 404 at once. `ShareLinks::create()`, `revoke()` and `for()` do the same from code; `shareLinks(false)` and `canShare(fn)` turn it off or say who may share.
- Projects: **Expenses** — subcontractors, licences, travel — with a cost and, optionally, an amount charged to the client. They are added to the project’s cost, revenue and margin, and charged expenses are their own lines in the billing export. `Expenses::add()` and `Expenses::for()` do the same from code.
- Projects: **Fixed price** in a project’s settings makes the agreed price its revenue instead of hours × rates; its hours are left out of the billing lines.
- Timesheets: **work types** (*Design*, *Development*, *Meetings*) per project or for every project. People add a row per project and work type on *My timesheet*; the work report lists hours by work type; billing can group **By work type** (`groupBy: 'work_type'`); hours of a non-billable work type are cost but not revenue and are left out of the billing lines. `WorkTypes::create()`, `for()` and `remove()` do the same from code.
