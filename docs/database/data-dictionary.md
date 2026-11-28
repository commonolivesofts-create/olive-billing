# Workbook field inventory and data dictionary baseline

Directly transcribed from `Olivesoft_Invoice_Billing_MVP.xlsx`, `Invoice MVP`, rows 6–34, columns F/G. This records source fields, not final SQL column definitions. Exact nullability, indexes, field types and technical additions must be reviewed before each migration.

| Requirement | Table(s) | Source fields or notes |
|---|---|---|
| MVP-01 | users | id, name, email, password_hash, is_active, last_login_at |
| MVP-02 | — | — |
| MVP-03 | company_settings | id, legal_name, address, state_code, postal_code, country_code, gstin, pan, cin, email, phone, logo_key, signature_key, gst_registered |
| MVP-04 | billing_settings; document_sequences | billing_settings: id, default_currency, default_due_days, terms, footer_notes; document_sequences: id, financial_year, document_type, prefix, padding, next_number |
| MVP-05 | bank_accounts | id, bank_name, account_holder, account_number, ifsc, swift, branch, upi_id, is_default, is_active |
| MVP-06 | tax_settings | id, irn_required, lut_reference, lut_valid_from, lut_valid_to, export_tax_treatment |
| MVP-07 | email_settings | id, smtp_host, smtp_port, encryption, smtp_username, secret_ref, from_name, from_email |
| MVP-08 | customers | id, customer_code, name, contact_name, email, phone, billing_address, country_code, state_code, postal_code, gstin, gst_type, currency, due_days, is_active |
| MVP-09 | services | id, code, name, description, sac_code, unit, default_rate, currency, tax_rate_id, is_active |
| MVP-10 | tax_rates | id, name, treatment, rate_percent, effective_from, effective_to, is_active |
| MVP-11 | invoices | Read invoice, payment and correction records. No separate list table. |
| MVP-12 | invoices | id, customer_id, billing_plan_id, source_invoice_id (nullable links), document_type, financial_year, invoice_no, invoice_date, due_date, po_reference, period_start, period_end, currency, fx_rate, place_of_supply, tax_treatment, bank_account_id, notes, terms, status |
| MVP-13 | invoice_lines | id, invoice_id, service_id (nullable), line_no, description, sac_code, unit, quantity, unit_rate, discount_amount, taxable_amount, gst_rate, cgst_amount, sgst_utgst_amount, igst_amount, line_total |
| MVP-14 | invoices | subtotal, discount_total, taxable_total, cgst_total, sgst_utgst_total, igst_total, round_off, grand_total, total_inr |
| MVP-15 | invoices; document_sequences | invoices: issued_at, cancelled_at, cancellation_reason, seller_snapshot, buyer_snapshot, bank_snapshot, tax_snapshot; sequences: next_number |
| MVP-16 | invoice_irn | id, invoice_id (nullable), adjustment_note_id (nullable), irn, ack_no, ack_at, signed_qr, signed_response_key, status, cancelled_at, cancellation_reference |
| MVP-17 | invoice_documents | id, invoice_id (nullable), adjustment_note_id (nullable), document_type, storage_key, file_hash, generated_at |
| MVP-18 | invoice_emails | id, invoice_id (nullable), adjustment_note_id (nullable), document_id, to_email, cc_email, subject, body, status, provider_message_id, sent_at, error_message |
| MVP-19 | billing_plans | id, customer_id, name, contract_ref, start_date, end_date, frequency, billing_timing, anchor_date, next_period_start, next_bill_date, currency, due_days, tax_treatment, place_of_supply, status, predecessor_plan_id (nullable), previous_invoice_reference |
| MVP-20 | billing_plan_lines | id, billing_plan_id, service_id, description, sac_code, unit, quantity, unit_rate, discount_amount, tax_rate_id |
| MVP-21 | billing_cycles | id, billing_plan_id, period_start, period_end, scheduled_date, invoice_id (nullable), status, generated_at, last_error |
| MVP-22 | billing_cycles; invoices; billing_plans | Use plan_id / invoice_id links; invoices.source_invoice_id; billing_plans.previous_invoice_reference |
| MVP-23 | billing_plans; billing_plan_events | plans: status, end_date, next_bill_date, predecessor_plan_id; events: id, billing_plan_id, action, effective_date, reason, created_at |
| MVP-24 | invoice_payments | id, invoice_id, receipt_date, received_amount, tds_amount, received_inr, payment_mode, reference, notes, status |
| MVP-25 | invoice_payments | reversed_at, reversal_reason; balance = issued invoice + issued debit notes − issued credit notes − active receipts − active TDS. |
| MVP-26 | adjustment_notes; adjustment_note_lines | notes: id, invoice_id, type, note_no, note_date, financial_year, reason, currency, taxable_total, tax_total, total, status, issued_at; lines: id, note_id, invoice_line_id, description, sac_code, taxable_amount, gst_rate, cgst_amount, sgst_utgst_amount, igst_amount, line_total |
| MVP-27 | invoice_documents; invoice_emails; invoice_irn | Reuse linked note ID in document, email and IRN records; invoice_id or adjustment_note_id, never both. |
| MVP-28 | — | Derived from invoices, billing_plans, billing_cycles, invoice_payments and adjustment_notes. |
| MVP-29 | — | Use existing records; no separate reporting tables. |

## Technical additions (design baseline)
- `sessions`: persistent session identifier, serialized session payload, idle/absolute expiry, authenticated user reference when applicable; exact adapter schema pending P02.
- `audit_logs`: actor, action, entity, record identifier, UTC event time, request ID and redacted change details; restrict modification/deletion.
- `idempotency_requests`: operation scope, key, payload fingerprint, processing/result state and timestamps; unique scope/key; transactional handling.
- `schema_migrations`: version, checksum, status and applied time; migration locking and failure/recovery metadata to be specified in P01.
- All stored tables receive created_at/updated_at per workbook; technical table adaptations and version field for ETag must be documented. No application schema is installed yet.
