# Architecture Blueprint

## 1) High-level components

1. **Form Builder Service**
   - Stores form schema and versions.
2. **Rules Engine**
   - Evaluates conditional logic at runtime.
3. **Submission Service**
   - Persists responses and validates schema.
4. **Spreadsheet Service**
   - Provides sheet-like CRUD over form data.
5. **Import Service**
   - Reads CSV/XLSX and maps to form schema.
6. **Access Control Service**
   - Enforces public/private form policies and respondent identity rules.

## 2) Suggested data model (PostgreSQL)

### `forms`
- `id` (uuid, pk)
- `name` (text)
- `description` (text)
- `status` (draft/published/closed)
- `access_mode` (public/private)
- `allow_anonymous` (bool)
- `created_by` (uuid)
- `created_at`, `updated_at`

### `form_access_policies`
- `id` (uuid, pk)
- `form_id` (uuid, fk forms.id)
- `policy_type` (email_allowlist/domain_allowlist/role)
- `policy_json` (jsonb)
- `created_at`

### `form_versions`
- `id` (uuid, pk)
- `form_id` (uuid, fk forms.id)
- `schema_json` (jsonb)
- `is_active` (bool)
- `created_at`

### `fields`
- `id` (uuid, pk)
- `form_version_id` (uuid, fk)
- `key` (text)
- `label` (text)
- `type` (text)
- `config_json` (jsonb)
- `order_index` (int)

### `field_rules`
- `id` (uuid, pk)
- `form_version_id` (uuid, fk)
- `target_field_key` (text)
- `rule_json` (jsonb)

### `lookup_tables`
- `id` (uuid, pk)
- `name` (text)
- `schema_json` (jsonb)

### `lookup_rows`
- `id` (uuid, pk)
- `lookup_table_id` (uuid, fk)
- `row_json` (jsonb)

### `submissions`
- `id` (uuid, pk)
- `form_id` (uuid, fk)
- `form_version_id` (uuid, fk)
- `respondent_id` (uuid nullable)
- `respondent_name` (text)
- `answer_json` (jsonb)
- `started_at` (timestamptz)
- `submitted_at` (timestamptz)
- `created_at`, `updated_at`

### `submission_locations`
- `submission_id` (uuid, pk, fk submissions.id)
- `latitude` (numeric)
- `longitude` (numeric)
- `accuracy_m` (numeric)
- `captured_at` (timestamptz)
- `source` (browser/manual)

### `submission_cells` (optional for fast sheet ops)
- `submission_id` (uuid, fk)
- `field_key` (text)
- `value_text` (text)
- `value_num` (numeric)
- `value_date` (timestamp)

## 3) Core API endpoints

- `POST /forms`
- `GET /forms`
- `POST /forms/:id/versions`
- `POST /forms/:id/publish`
- `POST /forms/:id/submissions`
- `GET /forms/:id/submissions`
- `PATCH /forms/:id/submissions/:submissionId`
- `POST /forms/:id/access-policies`
- `GET /forms/:id/access-policies`
- `POST /imports/forms/from-sheet`
- `GET /imports/templates/form`

## 4) Conditional logic execution flow

1. Client loads active `form_version` schema.
2. On field change, client sends answer state to rules engine.
3. Rules engine returns UI state diff:
   - visible fields
   - required fields
   - computed defaults
4. Client rerenders incrementally.

## 5) Spreadsheet flow

1. Grid queries submissions as row set.
2. Default system columns include respondent name, submitted time, and submission id.
3. Inline edit creates patch request.
4. Backend validates against form schema + rules.
5. Persist and emit audit log event.

## 6) Geolocation capture flow

1. Form field configured as `location` with capture mode (`browser`, `manual`, or `both`).
2. Browser requests geolocation permission for respondent.
3. On consent, client sends coordinates + accuracy with submission payload.
4. Backend validates and stores in `submission_locations`.
5. Spreadsheet view can display location columns and map links.
