# Product Requirements: FormPlus

## 1) Problem statement
Organizations need a single system where admins can:

1. Build rich forms with complex logic.
2. Collect submissions from end users.
3. Work with collected data in spreadsheet-like views.
4. Generate forms from existing spreadsheet data.

## 2) Personas

- **Admin**: designs forms, manages structure, views and edits data.
- **Respondent/User**: fills in and submits forms.

## 3) Functional requirements

### 3.1 Admin form management
- Create, duplicate, archive, and delete multiple forms.
- Form-level settings:
  - title, description, status (draft/published/closed),
  - submit behavior,
  - access policy (public/private),
  - response collection settings (capture respondent name, capture submit timestamp).

### 3.2 Public and private access modes
- **Public forms**:
  - accessible through shareable links,
  - optional anonymous responses,
  - optional CAPTCHA/rate limiting.
- **Private forms**:
  - require authentication,
  - support allowlist by email/domain,
  - optional role-based restriction.

### 3.3 Form builder features
- Field types:
  - single-line text, paragraph, number, email, phone,
  - date/time,
  - single select, multi-select,
  - cascading dropdown,
  - checkbox, radio,
  - file upload,
  - location/geolocation (auto-detect via browser with permission + manual fallback),
  - calculated field (formula).
- Validation:
  - required,
  - min/max length,
  - min/max value,
  - regex,
  - custom error message.
- Conditional logic:
  - show/hide field based on prior answer,
  - make field required conditionally,
  - set default value conditionally,
  - skip to section/end.

### 3.4 Cascading dropdowns
- Admin defines hierarchy and parent-child relationships.
- Child options filtered by parent selected value.
- Option source can come from manual entries or spreadsheet table.

### 3.5 Spreadsheet-driven form creation
- Upload CSV/XLSX or select internal sheet.
- Generate fields from column headers.
- Infer field types from sample values.
- Optional mapping wizard for type overrides and validation setup.
- Provide downloadable sample spreadsheet templates to bootstrap imports.

### 3.6 Submission handling
- End users submit published forms.
- Each submission stores:
  - form id,
  - version id,
  - respondent metadata (name, user id if authenticated),
  - answer payload,
  - geolocation coordinates when enabled and consented,
  - timestamps (started_at, submitted_at).

### 3.7 Built-in spreadsheet system
- Grid view per form for all submissions.
- Features:
  - create/edit/delete rows,
  - filter, sort, group,
  - inline edits with validation,
  - formulas,
  - export CSV/XLSX.
- Admins can edit submission rows directly in sheet.
- Default visible system columns: respondent name, form fill time, submission id.

## 4) Non-functional requirements
- Role-based access control.
- Audit logs for admin edits.
- Autosave drafts in builder.
- API-first architecture.
- Horizontal scalability for submissions.

## 5) MVP scope
- Multi-form management.
- Public/private form access controls.
- Builder with common field types + location capture.
- Conditional logic (show/hide + required).
- Cascading dropdown from lookup table.
- Submission capture with respondent name + submitted timestamp.
- Submission capture and spreadsheet grid view.
- CSV import to bootstrap form structure using sample template.

## 6) Phase 2
- Advanced calculations and cross-form joins.
- Workflow automations (notifications/webhooks).
- External integrations (Google Sheets, Airtable, CRM).
- AI-assisted form generation.
