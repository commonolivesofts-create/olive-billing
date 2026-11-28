# ITSAZZ UI/UX standards

Step 3.6 approved. Exact visual reference/assets remain to be reviewed; this is a design baseline, not a reproduction of an existing implementation.

- Enterprise, Oracle Redwood-inspired, compact, elegant, simple, responsive, accessible. No generic template, forced green, decorative cards, excessive scrolling or invented functionality.
- Horizontal main navigation: Dashboard, Customers, Invoices, Billing Plans, Payments, Masters, Reports, Settings. Submenus only approved functions. Mobile collapsible navigation, tablet adaptive layouts, desktop compact layout. Header, breadcrumb, page title, relevant actions and minimal footer.
- Central design tokens: primary #345B83, nav #253B56, background #F5F7FA, surface #FFFFFF, text #1F2937, muted #64748B, border #DCE2E9, success #15803D, warning #B45309, error #B91C1C. Proposed Inter/system fallback. Compare against authorized reference before final visuals; verify contrast.
- Page heading 22px; section 16px; labels/table 13px; inputs 14px; body support 12px. Spacing 4/8/12/16/24/32px; desktop padding 16–24px; controls 36–40px; rows 40–44px; accessible mobile touch targets. Use tokens and adaptive layout.
- Reuse Button, Input, Select, SearchableSelect, DatePicker, CurrencyInput, FormField, DataTable, Pagination, SearchBar, FilterPanel, StatusIndicator, Modal, ConfirmDialog, Toast, LoadingSkeleton, EmptyState, ErrorState, PageHeader, Breadcrumb, AppHeader, HorizontalNav, ProtectedRoute as appropriate.
- Visible labels, required indicators, inline field errors, preservation of failed form input, unsaved-change guard, processing state, consistent save/cancel, accessible keyboard and focus behavior. Server-side search with debounce; default 25 rows, max 100; only supported filter/sort fields; right-align amounts; readable status text, not colour alone.
- Invoice editor: approved header and lines, backend tax preview, currency, save draft and controlled issue; issued fields read-only except authorized actions. Do not fabricate totals, dashboard numbers, or successful operations. Display errors safely. Financial confirmations and duplicate-submission guards required.
- Acceptance: requirement traceability, actual API integration, loading/empty/errors, responsiveness, accessibility, tests and independent PR review. Reference screenshots/logo and invoice PDF layout remain open.
