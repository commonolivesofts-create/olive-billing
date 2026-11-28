# Approved Invoice & Billing MVP

Baseline: 28 November 2026. Business scope approved in Step 3.1. Source: `Olivesoft_Invoice_Billing_MVP.xlsx`, sheet `Invoice MVP`, rows 6–34 and 37–43. The five older documents are supporting references; this workbook wins on business scope. The approved horizontal navigation overrides row 7.

## Requirements traceability

### MVP-01 — Login (P02)

- Module: Setup
- Feature: Sign in, sign out and change password.
- Form fields: Email, password, current password, new password.
- Database reference: users

### MVP-02 — Application layout (P01)

- Module: Setup
- Feature: Sidebar, header and invoice-focused navigation.
- Form fields: —
- Database reference: —
- Approved override: horizontal main navigation, responsive layout; no permanent desktop sidebar.

### MVP-03 — Company profile form (P03)

- Module: Company settings
- Feature: Save one company, GST details and branding.
- Form fields: Legal name, billing address, state, PIN, country, GSTIN, PAN, CIN, email, phone, logo, signature, GST registration status.
- Database reference: company_settings

### MVP-04 — Invoice settings form (P03)

- Module: Company settings
- Feature: Set FY numbering, default terms, currency and invoice footer.
- Form fields: Financial year, invoice prefix, starting number, padding, credit/debit note prefixes, due days, currency, terms, notes.
- Database reference: billing_settings; document_sequences

### MVP-05 — Bank details form (P03)

- Module: Company settings
- Feature: Show payment instructions on invoices. One default bank.
- Form fields: Bank name, account holder, account number, IFSC, SWIFT (if needed), branch, UPI ID (optional).
- Database reference: bank_accounts

### MVP-06 — GST / export settings form (P03)

- Module: Company settings
- Feature: Set applicable tax treatments; confirm IRN requirement with CA.
- Form fields: IRN applicability, LUT reference and validity (if used), export tax treatment.
- Database reference: tax_settings

### MVP-07 — Email settings form (P03)

- Module: Company settings
- Feature: Set SMTP and send a test email.
- Form fields: Host, port, encryption, username, password, from name, from email, test recipient.
- Database reference: email_settings

### MVP-08 — Customer list / customer form (P04)

- Module: Masters
- Feature: Add, edit, search and deactivate customers.
- Form fields: Customer name, contact person, email, phone, address, country, state, PIN, GSTIN, GST type, currency, due days.
- Database reference: customers

### MVP-09 — Service list / service form (P04)

- Module: Masters
- Feature: Maintain software, AMC, subscription and consulting services.
- Form fields: Service code, name, invoice description, SAC, unit, default rate, currency, tax code, active.
- Database reference: services

### MVP-10 — Tax rates list / form (P04)

- Module: Masters
- Feature: Maintain approved GST rates and non-taxable classifications.
- Form fields: Tax name, treatment, GST %, effective from, effective to, active.
- Database reference: tax_rates

### MVP-11 — Invoice list (P05)

- Module: Invoicing
- Feature: Search invoices by number, customer, date, FY and status.
- Form fields: Search, customer, date range, FY, document status, payment status.
- Database reference: invoices

### MVP-12 — New / edit invoice — header (P05)

- Module: Invoicing
- Feature: Create one-time invoices or invoices for a billing period.
- Form fields: Customer, invoice type/date, due date, PO/reference, service period, currency, exchange rate, place of supply, tax treatment, bank, notes.
- Database reference: invoices

### MVP-13 — New / edit invoice — service lines (P05)

- Module: Invoicing
- Feature: Add services, quantities, rates, discounts and tax.
- Form fields: Service, description, SAC, unit, quantity, rate, discount amount, tax rate.
- Database reference: invoice_lines

### MVP-14 — Invoice totals / preview (P05)

- Module: Invoicing
- Feature: Calculate totals on server; support domestic, non-GST and approved export cases.
- Form fields: Subtotal, discount, taxable amount, CGST, SGST/UTGST, IGST, round-off, total (calculated).
- Database reference: invoices

### MVP-15 — Invoice detail / issue (P06)

- Module: Invoicing
- Feature: Assign final FY number once; lock issued data; allow draft editing and permitted cancellation.
- Form fields: Issue confirmation, cancellation reason (if permitted).
- Database reference: invoices; document_sequences

### MVP-16 — GST e-invoice details (if applicable) (P06)

- Module: Invoicing
- Feature: Use approved IRP process; block final release until valid IRN / QR. No portal integration assumed.
- Form fields: IRN, acknowledgment no./date, signed QR / response, status, cancellation reference.
- Database reference: invoice_irn

### MVP-17 — Invoice PDF / print preview (P07)

- Module: PDF & email
- Feature: One branded A4 template. Generate, store, print and download final PDF.
- Form fields: No extra form; use issued invoice and saved branding.
- Database reference: invoice_documents

### MVP-18 — Send invoice / email history (P07)

- Module: PDF & email
- Feature: Send PDF to customer; resend and view success or failure.
- Form fields: To, CC, subject, message, PDF attachment (auto).
- Database reference: invoice_emails

### MVP-19 — Billing plan list / plan form (P08)

- Module: Recurring billing
- Feature: Set up AMC, subscription or retainer billing. Monthly, quarterly or yearly.
- Form fields: Customer, plan name, contract/PO ref, start date, end date (optional), frequency, billing timing, first period, next billing date, currency, due days, tax treatment, place of supply, active.
- Database reference: billing_plans

### MVP-20 — Billing plan — service lines (P08)

- Module: Recurring billing
- Feature: Save standard invoice lines for each billing cycle.
- Form fields: Service, description, SAC, unit, quantity, rate, discount, GST code.
- Database reference: billing_plan_lines

### MVP-21 — Upcoming billing / generate next invoice (P09)

- Module: Recurring billing
- Feature: Auto-create one draft per due period; manual generate/retry. Never issue or email automatically.
- Form fields: Due date filter, plan, generate / retry (actions).
- Database reference: billing_cycles

### MVP-22 — Billing plan detail / invoice chain (P09)

- Module: Recurring billing
- Feature: Show all periods and linked invoices; open draft, PDF or payment; duplicate prior one-time invoice as new draft.
- Form fields: Period filter, invoice status, previous invoice reference (optional for a new plan).
- Database reference: billing_cycles; invoices; billing_plans

### MVP-23 — Pause / resume / renew plan (P09)

- Module: Recurring billing
- Feature: Pause, resume or renew; create a linked new plan for changed rates. Keep older invoices unchanged.
- Form fields: Action, effective date, new end date, next rate (if renewed), reason.
- Database reference: billing_plans; billing_plan_events

### MVP-24 — Invoice detail — record payment (P10)

- Module: Payments
- Feature: Record full or partial receipts; support invoice-level TDS if applicable.
- Form fields: Receipt date, amount received (invoice currency), TDS amount, payment mode, reference, notes; actual INR received if foreign currency.
- Database reference: invoice_payments

### MVP-25 — Payment history / reversal (P10)

- Module: Payments
- Feature: View receipts and reverse incorrect entries; do not delete posted receipts.
- Form fields: Payment, reversal date, reason.
- Database reference: invoice_payments

### MVP-26 — Credit / debit note form & detail (P11)

- Module: Invoice corrections
- Feature: Correct issued invoices with linked notes where required; no editing issued invoices.
- Form fields: Invoice, note type, date, reason, affected line, quantity/value, tax adjustment.
- Database reference: adjustment_notes; adjustment_note_lines

### MVP-27 — Note PDF / send (P11)

- Module: Invoice corrections
- Feature: Generate and share linked correction note. Apply IRN if required.
- Form fields: Recipient, subject, message.
- Database reference: invoice_documents; invoice_emails; invoice_irn

### MVP-28 — Billing dashboard (P12)

- Module: Billing overview
- Feature: Show upcoming renewals, due drafts, issued, unpaid and overdue invoices.
- Form fields: Date range, customer, currency.
- Database reference: —

### MVP-29 — Customer billing history (P12)

- Module: Billing overview
- Feature: View customer invoice chain, paid and outstanding totals; export invoice list.
- Form fields: Customer, FY, service period, status, currency.
- Database reference: —

## Mandatory implementation rules

- **Implementation rules:** Single admin role; one initial admin account. No role/permission master, vendor, expense, inventory, payroll or general ledger.
- **Invoice integrity:** Assign numbers on issue only; unique by financial year and document series. Retain cancelled numbers and immutable issued snapshots. GST series max. 16 characters.
- **Recurring invoice rule:** Unique cycle per plan + period_start; generate drafts only. Advance schedule after successful draft creation; retain missed cycles; review tax/rate/FX before issue.
- **Payment rule:** Use decimal amounts. Reject excess settlement; no advances or multi-invoice allocations in this MVP. Show currencies separately; overdue is based on due date and positive balance.
- **Database conventions:** PK: id; FK: \*\_id. Add created_at / updated_at to stored tables. Use secure password hashes, encrypted SMTP secrets, private PDF storage and backups.
- **GST checks before go-live:** Confirm registration, approved tax treatment, export/LUT and IRN applicability with CA. Do not assume overseas invoices are zero-rated. If mandated, IRP registration is a release requirement.

Original statutory reference links (not independently verified here): https://cbic-gst.gov.in/gst-invoice-rules.html | https://einvoice6.gst.gov.in/content/einvoice-mandate/ ;

No product inventory, supplier, purchase, expense, payroll, general ledger, configurable role management, customer advances or multi-invoice allocation. One company and one initial administrator.
