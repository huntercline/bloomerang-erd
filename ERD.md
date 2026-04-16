# Bloomerang CRM — Entity Relationship Diagram

Select a domain to explore the schema. Tables show key columns; relationships listed below.

---

## Constituents

Core constituent (Account) record and all associated contact information, household membership, and relationships between accounts.

### Tables

**Account**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Type | |
| varchar | Status | |
| varchar | NameFirst | |
| varchar | NameLast | |
| varchar | NameSort | |
| bigint | RelationshipManagerId | FK |
| decimal | LifetimeGiving | |

**Address**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | AddressTypeId | FK |
| varchar | Street | |
| varchar | City | |
| varchar | State | |
| varchar | PostalCode | |
| tinyint | IsPrimary | |

**AddressType**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**Email**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | EmailTypeId | FK |
| varchar | Value | |
| tinyint | IsPrimary | |

**EmailType**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**Phone**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | PhoneTypeId | FK |
| varchar | Number | |
| tinyint | IsPrimary | |

**PhoneType**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**HouseholdMembership**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | HouseholdId | FK |
| tinyint | IsPrimary | |

**Relationship**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId1 | FK |
| bigint | AccountId2 | FK |
| bigint | RelationshipRoleId1 | FK |
| bigint | RelationshipRoleId2 | FK |

**RelationshipRole**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**AccountDuplicate**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId1 | FK |
| bigint | AccountId2 | FK |
| tinyint | Dismissed | |

**UserAccount**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Username | |
| varchar | FullName | |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| Account | 1:N → | Address | has |
| Address | N:1 → | AddressType | typed by |
| Account | 1:N → | Email | has |
| Email | N:1 → | EmailType | typed by |
| Account | 1:N → | Phone | has |
| Phone | N:1 → | PhoneType | typed by |
| Account | 1:N → | HouseholdMembership | member via |
| Account | 1:N → | Relationship | AccountId1 / AccountId2 |
| Relationship | N:1 → | RelationshipRole | role 1 & 2 |
| Account | 1:N → | AccountDuplicate | AccountId1 / AccountId2 |
| Account | N:1 → | UserAccount | relationship mgr |

---

## Transactions

The full giving lifecycle: transaction headers, designations (the unit of giving), funds, campaigns, appeals, tributes, soft credits, recurring schedules, pledge installments, and refunds.

### Tables

**Account**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | NameFirst | |
| varchar | NameLast | |

**TransactionHeader**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | WidgetId | FK |
| datetime | Date | |
| decimal | Amount | |
| varchar | Type | |

**Designation**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | TransactionId | FK |
| bigint | FundId | FK |
| bigint | CampaignId | FK |
| bigint | AppealId | FK |
| bigint | TributeId | FK |
| bigint | PledgeId | FK |
| bigint | RecurringDonationId | FK |
| decimal | Amount | |
| varchar | Type | |

**Fund**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**Campaign**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**Appeal**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**Tribute**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | TributeTypeId | FK |
| varchar | Name | |

**TributeType**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**SoftCredit**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | DesignationId | FK |
| bigint | AccountId | FK |
| bigint | PledgeId | FK |
| decimal | Amount | |

**RecurringDonationSchedule**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | RecurringDonationId | FK |
| varchar | Frequency | |
| decimal | Amount | |
| date | NextPaymentDate | |

**PledgeInstallment**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | PledgeId | FK |
| date | DueDate | |
| decimal | Amount | |

**Refund**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | TransactionId | FK |
| decimal | Amount | |
| datetime | Date | |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| Account | 1:N → | TransactionHeader | gives |
| TransactionHeader | 1:N → | Designation | contains |
| Designation | N:1 → | Fund | to fund |
| Designation | N:1 → | Campaign | via campaign |
| Designation | N:1 → | Appeal | via appeal |
| Designation | N:1 → | Tribute | honors |
| Tribute | N:1 → | TributeType | typed by |
| Designation | 1:N → | SoftCredit | soft credited via |
| Account | 1:N → | SoftCredit | soft credited to |
| Designation | 1:N → | RecurringDonationSchedule | scheduled by |
| Designation | 1:N → | PledgeInstallment | pledge installment |
| TransactionHeader | 1:N → | Refund | refunded via |

---

## Engagement

Everything that happens between transactions: interactions, notes, tasks, groups, file attachments, engagement scoring, and website visits.

### Tables

**Account**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | NameFirst | |
| varchar | NameLast | |
| int | EngagementLevelScore | |

**Interaction**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| varchar | Channel | |
| varchar | Subject | |
| datetime | Date | |

**Note**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| text | Text | |
| datetime | Date | |

**Task**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | InteractionId | FK |
| bigint | UserId | FK |
| varchar | Subject | |
| date | DueDate | |
| tinyint | IsCompleted | |

**Group**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |
| varchar | Type | |

**GroupAccount**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | GroupId | FK |
| bigint | AccountId | FK |
| date | JoinDate | |

**FileAttachment**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | InteractionId | FK |
| bigint | NoteId | FK |
| bigint | TaskId | FK |
| varchar | FileName | |

**EngagementRecord**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| varchar | Type | |
| int | Score | |
| date | Date | |

**WebsiteVisit**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| datetime | VisitDate | |
| varchar | Url | |

**UserAccount**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Username | |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| Account | 1:N → | Interaction | logged for |
| Account | 1:N → | Note | noted on |
| Account | 1:N → | Task | assigned to |
| Interaction | 1:N → | Task | linked to |
| Task | N:1 → | UserAccount | owned by |
| Account | 1:N → | GroupAccount | member via |
| Group | 1:N → | GroupAccount | contains |
| Account | 1:N → | FileAttachment | attached to |
| Interaction | 1:N → | FileAttachment | attached to |
| Note | 1:N → | FileAttachment | attached to |
| Task | 1:N → | FileAttachment | attached to |
| Account | 1:N → | EngagementRecord | scored via |
| Account | 1:N → | WebsiteVisit | tracked for |

---

## Email & Comms

Email template → scheduled send → mailing → recipient pipeline, tracked links, email interest subscriptions, and letter mailings.

### Tables

**EmailTemplate**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |
| varchar | Subject | |
| varchar | Type | |

**EmailScheduled**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | EmailTemplateId | FK |
| bigint | UserAccountId | FK |
| bigint | ScheduledFrequencyId | FK |
| datetime | ScheduledDate | |

**EmailMailing**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | EmailTemplateId | FK |
| bigint | EmailScheduledId | FK |
| datetime | SentDate | |

**EmailMailingRecipient**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | EmailMailingId | FK |
| bigint | AccountId | FK |
| varchar | Status | |

**EmailMailingTrackedLink**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | EmailMailingId | FK |
| varchar | Url | |

**EmailInterest**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**AccountEmailInterest**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | EmailInterestId | FK |

**Account**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | NameFirst | |
| varchar | NameLast | |
| varchar | EmailInterestType | |

**LetterTemplate**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |
| varchar | Type | |

**LetterMailing**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | LetterTemplateId | FK |
| datetime | SentDate | |

**Widget**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | CampaignId | FK |
| bigint | AppealId | FK |
| bigint | EmailTemplateId | FK |
| varchar | Type | |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| EmailTemplate | 1:N → | EmailScheduled | scheduled by |
| EmailTemplate | 1:N → | EmailMailing | sent via |
| EmailScheduled | 1:N → | EmailMailing | triggered |
| EmailMailing | 1:N → | EmailMailingRecipient | sent to |
| EmailMailing | 1:N → | EmailMailingTrackedLink | tracks |
| Account | 1:N → | EmailMailingRecipient | receives |
| Account | 1:N → | AccountEmailInterest | interested in |
| EmailInterest | 1:N → | AccountEmailInterest | subscribed via |
| LetterTemplate | 1:N → | LetterMailing | printed via |
| Widget | N:1 → | EmailTemplate | uses template |

---

## Custom Fields

The Field → Value → ValueAssignment pattern used for custom data on both accounts and transactions. Field definitions live in categories; values are the predefined options; assignments link selected values to records.

### Tables

**CustomAccountFieldCategory**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**CustomAccountField**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | CategoryId | FK |
| varchar | Name | |
| varchar | Type | |

**CustomAccountValue**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | FieldId | FK |
| varchar | Value | |

**CustomAccountValueAssignments**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | CustomAccountValueId | FK |

**CustomTransactionFieldCategory**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**CustomTransactionField**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | CategoryId | FK |
| varchar | Name | |
| varchar | Type | |

**CustomTransactionValue**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | DesignationId | FK |
| bigint | FieldId | FK |
| varchar | Value | |

**CustomTransactionValueAssignments**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | DesignationId | FK |
| bigint | CustomTransactionValueId | FK |

**Account**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | NameFirst | |

**Designation**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| decimal | Amount | |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| CustomAccountFieldCategory | 1:N → | CustomAccountField | groups |
| CustomAccountField | 1:N → | CustomAccountValue | valued by |
| Account | 1:N → | CustomAccountValue | has |
| Account | 1:N → | CustomAccountValueAssignments | assigned via |
| CustomAccountValue | 1:N → | CustomAccountValueAssignments | selected in |
| CustomTransactionFieldCategory | 1:N → | CustomTransactionField | groups |
| CustomTransactionField | 1:N → | CustomTransactionValue | valued by |
| Designation | 1:N → | CustomTransactionValue | has |
| Designation | 1:N → | CustomTransactionValueAssignments | assigned via |
| CustomTransactionValue | 1:N → | CustomTransactionValueAssignments | selected in |

---

## Memberships

Membership programs, levels (each linked to a Group for access control), schedules tied to designations, and payment failure tracking.

### Tables

**MembershipProgram**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |
| varchar | Description | |

**MembershipProgramLevel**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | MembershipProgramId | FK |
| bigint | GroupId | FK |
| varchar | Name | |
| decimal | Amount | |
| varchar | BillingFrequency | |

**MembershipSchedule**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | MembershipDesignationId | FK |
| bigint | MembershipProgramLevelId | FK |
| varchar | Status | |
| date | StartDate | |
| date | EndDate | |

**Designation**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | TransactionId | FK |
| decimal | Amount | |
| varchar | Type | |

**Group**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**MembershipPaymentFailure**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | MembershipScheduleId | FK |
| bigint | WalletItemId | FK |
| datetime | FailedDate | |
| varchar | Reason | |

**WalletItem**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| varchar | PaymentMethodType | |
| varchar | LastFour | |

**Widget**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | MembershipProgramId | FK |
| varchar | Type | |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| MembershipProgram | 1:N → | MembershipProgramLevel | has levels |
| MembershipProgramLevel | N:1 → | Group | grants group |
| MembershipProgramLevel | 1:N → | MembershipSchedule | on schedule |
| Designation | 1:N → | MembershipSchedule | fulfills |
| MembershipSchedule | 1:N → | MembershipPaymentFailure | fails via |
| MembershipPaymentFailure | N:1 → | WalletItem | tried with |
| MembershipProgram | 1:N → | Widget | offered on |

---

## Payments

Payment processor accounts, stored wallet items (saved payment methods), processing info per transaction, and payment failure logs across recurring and pledge flows.

### Tables

**TransactionProcessorAccount**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | ProcessorType | |
| varchar | Name | |
| varchar | StripeAccountId | |

**WalletItem**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| bigint | TransactionProcessorAccountId | FK |
| varchar | PaymentMethodType | |
| varchar | LastFour | |
| date | ExpiryDate | |

**ProcessingInfo**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | TransactionId | FK |
| bigint | TransactionProcessorAccountId | FK |
| varchar | ProcessorTransactionId | |
| decimal | Fee | |

**TransactionHeader**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AccountId | FK |
| decimal | Amount | |
| varchar | Type | |

**RecurringDonationPaymentFailure**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | RecurringDonationScheduleId | FK |
| bigint | WalletItemId | FK |
| datetime | FailedDate | |
| varchar | Reason | |

**RecurringDonationSchedule**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | RecurringDonationId | FK |
| date | NextPaymentDate | |

**PledgePaymentFailure**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | PledgeId | FK |
| bigint | PledgeInstallmentId | FK |
| bigint | WalletItemId | FK |
| datetime | FailedDate | |

**PayoutSchedule**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | TransactionProcessorAccountId | FK |
| varchar | Frequency | |

**Account**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | NameFirst | |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| Account | 1:N → | WalletItem | stores |
| TransactionProcessorAccount | 1:N → | WalletItem | processed by |
| TransactionProcessorAccount | 1:N → | ProcessingInfo | processes via |
| TransactionHeader | 1:N → | ProcessingInfo | processed in |
| RecurringDonationSchedule | 1:N → | RecurringDonationPaymentFailure | fails via |
| WalletItem | 1:N → | RecurringDonationPaymentFailure | tried with |
| WalletItem | 1:N → | PledgePaymentFailure | tried with |
| TransactionProcessorAccount | 1:N → | PayoutSchedule | paid out via |

---

## Admin & System

Users, roles, permissions, import templates and executions, reports, scheduled reports, mass updates, and system alerts.

### Tables

**UserAccount**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Username | |
| varchar | Email | |
| varchar | FullName | |
| tinyint | IsActive | |

**Role**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |

**RoleUser**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | UserId | FK |
| bigint | RoleId | FK |

**RolePermission**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | UserId | FK |
| bigint | RoleId | FK |
| varchar | Permission | |

**ImportTemplate**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |
| varchar | EntityType | |

**ImportExecution**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | ImportTemplateId | FK |
| datetime | ExecutedDate | |
| int | RecordsImported | |

**Report**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Name | |
| varchar | Type | |

**ReportScheduled**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | ReportId | FK |
| bigint | UserAccountId | FK |
| varchar | Frequency | |

**MassUpdateExecution**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | EntityType | |
| datetime | ExecutedDate | |

**Alert**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| varchar | Message | |
| datetime | CreatedDate | |

**AlertAudience**

| Type | Column | Key |
|------|--------|-----|
| bigint | Id | PK |
| bigint | AlertId | FK |
| bigint | UserAccountId | FK |

### Relationships

| From | Cardinality | To | Description |
|------|-------------|-----|-------------|
| UserAccount | 1:N → | RoleUser | assigned via |
| Role | 1:N → | RoleUser | granted via |
| UserAccount | 1:N → | RolePermission | has |
| Role | 1:N → | RolePermission | defines |
| ImportTemplate | 1:N → | ImportExecution | run via |
| Report | 1:N → | ReportScheduled | scheduled via |
| UserAccount | 1:N → | ReportScheduled | receives |
| Alert | 1:N → | AlertAudience | sent to |
| UserAccount | 1:N → | AlertAudience | receives |

---

*Generated from combined_schema.sql · MySQL 8.0 · Each customer schema shares this identical table structure*
