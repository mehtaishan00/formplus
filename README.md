# FormPlus

FormPlus is a multi-tenant form builder and spreadsheet-native data collection platform.

## Core capabilities

- **Multi-form administration**: admins can create and manage multiple forms.
- **Advanced form builder**: text, number, date, file, choice fields, validation rules, conditional visibility/required logic, and geolocation capture.
- **Cascading dropdowns**: dependent choice lists (e.g., Country → State → City) driven by relational data.
- **Public and private forms**:
  - publish public forms with shareable links,
  - restrict private forms to authenticated users, invited users, or domain-based access.
- **Spreadsheet-powered workflows**:
  - generate form fields from spreadsheet headers,
  - import option values from sheets,
  - view and edit submissions in a built-in spreadsheet UI,
  - start quickly from a sample spreadsheet template.
- **Response management**: each form writes to its own submission table and sheet view, including respondent name and form-fill timestamp.

## Suggested technical stack

- **Frontend**: Next.js + React + TypeScript.
- **Backend**: Node.js (NestJS or Next API routes) with REST/GraphQL.
- **Database**: PostgreSQL.
- **Spreadsheet grid**: AG Grid / TanStack Table + formula engine.
- **Rules engine**: JSON logic evaluator for conditional form behavior.

For implementation details, see:

- `docs/product-requirements.md`
- `docs/architecture.md`
- `docs/samples/form-import-template.csv`
