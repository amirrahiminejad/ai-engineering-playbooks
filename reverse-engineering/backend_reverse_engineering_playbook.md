# Playbook مهندسی معکوس Backend با کمک هوش مصنوعی

## ۱. هدف

این دستورالعمل برای تحلیل یک پروژه Backend موجود با زبان برنامه‌نویسی، Framework و معماری نامشخص تدوین شده است.

اهداف:

- شناسایی ساختار و فناوری‌های پروژه
- استخراج معماری واقعی سیستم از روی سورس
- شناسایی قابلیت‌ها، قواعد کسب‌وکار و جریان‌های اجرا
- تحلیل مدل داده و وابستگی‌های داخلی و خارجی
- مستندسازی رفتارهای موجود و ریسک‌های تغییر
- ایجاد امکان توسعه امن قابلیت‌های جدید
- آماده‌سازی نقشه راه بهبود معماری و بازنویسی تدریجی

## ۲. اصول اجرایی

1. تحلیل را از بررسی فقط‌خواندنی سورس شروع کن.
2. در مراحل کشف و مستندسازی، کد برنامه را تغییر نده.
3. زبان برنامه‌نویسی، Framework و معماری را از شواهد موجود استخراج کن؛ هیچ فناوری یا الگوی معماری را از پیش فرض نکن.
4. میان واقعیت مشاهده‌شده، رفتار تأییدشده در زمان اجرا، استنباط و مجهولات تفاوت بگذار.
5. برای یافته‌های مهم، مسیر فایل و نام کلاس، تابع یا تنظیمات مرتبط را ثبت کن.
6. نام فایل‌ها و پوشه‌ها را به‌تنهایی دلیل وجود یک قابلیت یا الگوی معماری در نظر نگیر.
7. تست‌ها، Build و رفتار Runtime را فقط زمانی موفق اعلام کن که واقعاً اجرا و بررسی شده باشند.
8. از اجرای دستورات مخرب، Migration، عملیات تغییر داده و اتصال به محیط Production بدون مجوز صریح خودداری کن.
9. مقادیر محرمانه، کلیدها، توکن‌ها و اطلاعات شخصی را در مستندات قرار نده.
10. تحلیل را مرحله‌به‌مرحله انجام بده. خروجی هر مرحله را پیش از شروع مرحله بعد اعتبارسنجی کن.
11. مستندات را بر اساس شواهد نگهداری کن و از تولید توضیحات غیرضروری درباره تک‌تک فایل‌ها و کلاس‌ها خودداری کن.
12. در صورت نبود شواهد کافی، موضوع را به‌عنوان مجهول ثبت کن و روش بررسی آن را مشخص کن.

## ۳. ساختار مستندات

تمام مستندات مهندسی معکوس را در مسیر زیر نگهداری کن:

```
docs/
└── reverse-engineering/
    ├── README.md
    ├── 01-repository-inventory.md
    ├── 02-execution-baseline.md
    ├── 03-entry-points.md
    ├── 03-api-catalog.md
    ├── 04-capability-map.md
    ├── 05-data-model.md
    ├── 05-dependency-map.md
    ├── 06-architecture-overview.md
    ├── 07-business-rules.md
    ├── 07-risk-register.md
    ├── 07-test-baseline.md
    ├── 08-validation-report.md
    ├── 10-modernization-roadmap.md
    ├── 10-prioritized-backlog.md
    ├── unknowns.md
    ├── diagrams/
    └── flows/
```

در صورت وجود مستندات قبلی، ابتدا آن‌ها را بررسی کن و از ایجاد فایل‌های تکراری یا بازنویسی اطلاعات معتبر خودداری کن.

## ۴. مرحله اول: شناسایی مخزن

### هدف

شناسایی فناوری‌ها، ساختار پروژه و روش اجرای برنامه، بدون فرض قبلی درباره زبان یا Framework.

### اقدامات

- بررسی ساختار مخزن و فایل‌های اصلی
- شناسایی زبان‌ها و نسخه‌های Runtime
- شناسایی Frameworkها، وابستگی‌ها و ابزارهای Build
- شناسایی Entry Pointهای برنامه
- بررسی فایل‌های تنظیمات و متغیرهای محیطی
- شناسایی دیتابیس‌ها، ORMها و ابزارهای Migration
- بررسی Docker، CI/CD و فایل‌های Deployment
- شناسایی ابزارهای تست و کدهای تولیدشده
- ثبت موارد ناشناخته و محدودیت‌های بررسی

### پرامپت

# Phase 1: Repository Inventory

Analyze the repository to identify its technology stack and structure without assuming a programming language, framework, or architecture.

Inspect:

- Languages and runtime versions
- Frameworks and dependencies
- Build tools and package managers
- Application entry points
- Configuration files
- Databases, ORM libraries, and migration tools
- Messaging, background jobs, and external integrations
- Tests and test frameworks
- Docker, CI/CD, and deployment configuration
- Generated and vendored code

For each finding, provide evidence using file paths and relevant symbols or configuration keys.

Distinguish confirmed facts, inferences, and unknowns.

Do not install dependencies, execute application code, or modify files.

Create `docs/reverse-engineering/01-repository-inventory.md`.

Include a summary of the technology stack, repository structure, execution entry points, external dependencies, risks, and unresolved questions.

Recommend the next investigation steps based on the detected technologies.

### معیار پذیرش

فناوری‌ها، ساختار کلی و نقاط ورود پروژه مشخص شده‌اند یا مجهولات آن‌ها مستند شده‌اند.

## ۵. مرحله دوم: بررسی Build و Runtime

### هدف

تعیین وضعیت واقعی ساخت، راه‌اندازی و تست پروژه.

### اقدامات

- بررسی دستورالعمل اجرای برنامه
- بررسی نسخه‌های موردنیاز ابزارها
- شناسایی تنظیمات و سرویس‌های خارجی موردنیاز
- بررسی اسکریپت‌های اجرا پیش از استفاده
- اجرای Build و تست‌ها در محیط ایزوله و با مجوز مناسب
- بررسی Startup و Health Check در صورت امکان
- ثبت خطاها و محدودیت‌های محیط اجرا

وضعیت هر بررسی را با یکی از مقادیر زیر ثبت کن:

- `PASS`: اجرا شده و موفق بوده است.
- `FAIL`: اجرا شده و ناموفق بوده است.
- `BLOCKED`: به دلیل وابستگی یا محدودیت محیط انجام نشده است.
- `NOT RUN`: هنوز اجرا نشده است.
- `UNKNOWN`: شواهد کافی وجود ندارد.

### پرامپت

# Phase 2: Execution Baseline

Determine whether the backend can be built, tested, and started safely.

Inspect the build and startup scripts before execution.

With appropriate approval and an isolated environment, verify:

- Required tool and runtime versions
- Dependency availability
- Build or compilation
- Existing automated tests
- Application startup
- Required configuration and external services
- Database connectivity and migration requirements
- Available health checks

Do not connect to production or execute destructive operations.

Record each check as PASS, FAIL, BLOCKED, NOT RUN, or UNKNOWN.

Include relevant error messages without exposing secrets.

Create:

- `docs/reverse-engineering/02-execution-baseline.md`
- `docs/reverse-engineering/unknowns.md`

Distinguish environment failures from application failures. Do not repair application code during this phase.

### معیار پذیرش

وضعیت Build، Startup، تست‌ها و وابستگی‌های محیطی مستند شده است.

## ۶. مرحله سوم: شناسایی نقاط ورود Backend

### هدف

شناسایی تمام مسیرهایی که اجرای منطق سیستم را آغاز می‌کنند.

### اقدامات

- استخراج Routeها و Endpointهای HTTP
- بررسی GraphQL، WebSocket و RPC در صورت وجود
- شناسایی Webhookها
- شناسایی Message Consumerها و Event Handlerها
- شناسایی Jobها و Scheduled Taskها
- شناسایی CLI و Commandهای مدیریتی
- بررسی Authentication و Authorization
- استخراج قرارداد ورودی و خروجی مسیرهای مهم

### پرامپت

# Phase 3: Backend Entry Points

Identify all execution entry points in the backend, including HTTP routes, GraphQL, WebSocket, RPC, webhooks, message consumers, event handlers, scheduled jobs, CLI commands, and internal service interfaces.

For each entry point, document:

- Identifier and trigger
- Method and path, topic, schedule, or command
- Handler file and symbol
- Input and output contracts
- Authentication and authorization
- Main downstream calls
- Data reads and writes
- External side effects
- Error handling
- Related tests
- Verification status and unknowns

Create:

- `docs/reverse-engineering/03-entry-points.md`
- `docs/reverse-engineering/03-api-catalog.md`

Do not assume that a declared route is reachable or functional without runtime evidence.

### معیار پذیرش

فهرست نقاط ورود و قراردادهای اصلی سیستم تهیه شده است.

## ۷. مرحله چهارم: استخراج قابلیت‌ها و جریان‌های اجرا

### هدف

شناخت رفتار واقعی سیستم از طریق ردیابی عملیات کسب‌وکار از نقطه ورود تا خروجی.

### اقدامات

1\. قابلیت‌های اصلی Backend را شناسایی کن.

2\. قابلیت‌ها را بر اساس اهمیت کسب‌وکار و ریسک اولویت‌بندی کن.

3\. برای هر قابلیت مهم، مسیر اجرای کد را دنبال کن.

4\. اعتبارسنجی‌ها، قواعد کسب‌وکار و تغییرات وضعیت را استخراج کن.

5\. عملیات خواندن و نوشتن داده را مشخص کن.

6\. فراخوانی سرویس‌های خارجی و عملیات ناهمگام را بررسی کن.

7\. Transactionها، Exceptionها، Retryها و Side Effectها را ثبت کن.

8\. مسیرهای موفقیت و شکست را مستند کن.

### پرامپت

# Phase 4: Capability and Execution Flow Recovery

Recover the actual business capabilities and execution flows of the backend.

Create a capability map based on source code, API contracts, tests, and available runtime evidence.

For each important capability, document:

- Business purpose
- Trigger and entry point
- Execution path with file paths and symbols
- Validation and business rules
- State transitions
- Data reads and writes
- Transaction boundaries
- External calls and asynchronous operations
- Side effects and error handling
- Existing tests and verification status
- Unresolved questions

Follow indirect calls and asynchronous processing where possible.

Create:

- `docs/reverse-engineering/04-capability-map.md`
- Relevant flow documents under `docs/reverse-engineering/flows/`

Produce Mermaid sequence diagrams for critical flows. Include only interactions supported by evidence.

Clearly identify incomplete execution paths and missing evidence.

### معیار پذیرش

جریان‌های مهم از نقطه ورود تا آخرین مرحله قابل مشاهده مستند شده‌اند.

## ۸. مرحله پنجم: تحلیل مدل داده و وابستگی‌ها

### هدف

شناسایی ساختار داده و وابستگی‌هایی که بر توسعه و تغییر سیستم اثر می‌گذارند.

### اقدامات

تحلیل داده:

- شناسایی دیتابیس‌ها، جدول‌ها، Collectionها و Entityها
- استخراج روابط، کلیدها، Constraintها و Indexها
- بررسی Migrationها و تغییرات Schema
- شناسایی Transactionها
- بررسی مالکیت داده و دسترسی مشترک بین ماژول‌ها
- بررسی حذف و ویرایش آبشاری
- شناسایی داده‌های حساس و نحوه استفاده از آن‌ها

تحلیل وابستگی:

- وابستگی Packageها و ماژول‌ها
- فراخوانی مستقیم و غیرمستقیم توابع
- وابستگی به Schema مشترک
- قراردادهای API و Event
- سرویس‌های خارجی و زیرساخت
- وابستگی‌های دوری و نقاط اتصال شدید

### پرامپت

# Phase 5: Data and Dependency Analysis

Analyze the backend's data model and dependency relationships.

Identify:

- Databases, tables, collections, and entities
- Relationships, keys, constraints, and indexes
- Migration history
- Transaction boundaries
- Shared database access
- Data ownership and mutation paths
- Module and package dependencies
- Direct and indirect calls
- API and event contracts
- External services and infrastructure dependencies
- Circular dependencies and tightly coupled components

For every important relationship, provide evidence from the repository.

Distinguish declared schema from runtime schema and verified dependencies from inferred ones.

Create:

- `docs/reverse-engineering/05-data-model.md`
- `docs/reverse-engineering/05-dependency-map.md`

Include ER diagrams and dependency diagrams where useful.

Do not execute schema-changing operations.

### معیار پذیرش

مدل داده و وابستگی‌های مهم هر قابلیت قابل ردیابی هستند.

## ۹. مرحله ششم: بازیابی معماری

### هدف

ایجاد تصویری مستند از معماری واقعی سیستم.

### اقدامات

- ترسیم System Context
- ترسیم Container Diagram
- ترسیم Component Diagram برای بخش‌های مهم
- مستندسازی ارتباطات داخلی و خارجی
- مشخص‌کردن مرزهای ماژول‌ها و سرویس‌ها
- شناسایی وابستگی‌های مشترک
- شناسایی محدودیت‌های معماری
- ثبت تفاوت معماری مشاهده‌شده با معماری مطلوب

### پرامپت

# Phase 6: Architecture Recovery

Recover the architecture that actually exists based on evidence gathered in previous phases.

Produce:

- C4 System Context diagram
- C4 Container diagram
- Component diagrams for critical components
- Dependency diagrams
- Data-flow and sequence diagrams for critical operations
- Architectural constraints and risk summary

For each component, document its responsibility, evidence, dependencies, data relationships, and runtime role when verified.

Distinguish observed architecture, inferred architecture, and desired future architecture.

Do not assume layered architecture, Clean Architecture, DDD, or microservices without supporting evidence.

Create:

- `docs/reverse-engineering/06-architecture-overview.md`
- Relevant diagrams under `docs/reverse-engineering/diagrams/`

Describe the actual structure even when it does not match a standard architectural pattern.

### معیار پذیرش

ساختار سیستم و ارتباط اجزای اصلی آن با شواهد مشخص شده است.

## ۱۰. مرحله هفتم: قواعد کسب‌وکار، ریسک‌ها و تست‌ها

### هدف

شناسایی رفتارهای حساس و تعیین میزان اطمینان لازم برای تغییر آن‌ها.

### اقدامات

- استخراج Business Ruleها از کد، تست‌ها و Constraintها
- بررسی کنترل دسترسی
- بررسی Transaction و سازگاری داده
- بررسی هم‌زمانی و Race Condition
- بررسی Idempotency و درخواست‌های تکراری
- بررسی Retry، Timeout و شکست سرویس‌های خارجی
- شناسایی عملیات غیرقابل بازگشت
- بررسی پوشش تست مسیرهای حیاتی
- تعریف تست‌های Characterization و Integration

### پرامپت

# Phase 7: Risk and Test Baseline

Analyze the backend's critical business rules, failure scenarios, and change risks.

Investigate:

- Business invariants
- Authentication and authorization
- Data consistency and transactions
- Concurrent operations and race conditions
- Idempotency and duplicate requests
- External-service failures, retries, and timeouts
- Irreversible operations
- Sensitive-data handling
- Existing test coverage

For each risk, document:

- Identifier
- Description and evidence
- Affected capabilities and components
- Likelihood and impact
- Existing safeguards
- Verification method
- Recommended mitigation
- Priority and status

Propose characterization and integration tests for critical flows.

Create:

- `docs/reverse-engineering/07-business-rules.md`
- `docs/reverse-engineering/07-risk-register.md`
- `docs/reverse-engineering/07-test-baseline.md`

Do not treat missing tests as proof of a defect. Do not change production logic during this phase. Report only test results that have actually been observed.

### معیار پذیرش

قواعد مهم، ریسک‌ها، تست‌های موجود و شکاف‌های تست مشخص شده‌اند.

## ۱۱. مرحله هشتم: اعتبارسنجی مستندات

### هدف

اطمینان از تطابق مستندات با سورس و شواهد واقعی.

### اقدامات

- بررسی وجود فایل‌ها و نمادهای ارجاع‌شده
- تطبیق دیاگرام‌ها با مسیرهای اجرا
- بررسی صحت روابط و وابستگی‌ها
- شناسایی تناقض بین مستندات و کد
- تفکیک یافته‌های قطعی از فرضیه‌ها
- بررسی نبود اطلاعات محرمانه
- بررسی صحت وضعیت Build و تست‌ها
- ثبت موارد ناقص و اصلاحات موردنیاز

### پرامپت

# Phase 8: Documentation Validation

Independently validate all artifacts under `docs/reverse-engineering/`.

Verify:

- Referenced files and symbols exist.
- Evidence supports each important claim.
- Source-level findings are distinguished from runtime verification.
- Diagrams match observed execution paths.
- Architectural labels are supported by evidence.
- Important flows and dependencies are not omitted.
- No secrets or sensitive information are exposed.
- Build and test results are accurate.

Do not silently convert uncertain findings into confirmed facts.

Create `docs/reverse-engineering/08-validation-report.md`.

For every issue, record its severity, affected artifact, supporting evidence, and required correction.

Do not modify application code.

### معیار پذیرش

مستندات بازبینی شده و موارد ناقص یا متناقض مشخص شده‌اند.

## ۱۲. مرحله نهم: فرایند توسعه قابلیت جدید

### هدف

استفاده از نتایج مهندسی معکوس برای توسعه کنترل‌شده محصول.

### اقدامات

1\. نیازمندی قابلیت جدید را مشخص کن.

2\. جریان‌های مرتبط و قواعد کسب‌وکار را پیدا کن.

3\. اثر تغییر بر API، داده، وابستگی‌ها و سرویس‌های خارجی را بررسی کن.

4\. تست‌های موردنیاز را مشخص کن.

5\. کوچک‌ترین راهکار قابل قبول را پیشنهاد بده.

6\. پس از تأیید طرح، تغییر را در محدوده مشخص پیاده‌سازی کن.

7\. تست‌ها و Build مرتبط را اجرا کن.

8\. Diff را بررسی کن و مستندات را به‌روز نگه دار.

### پرامپت

# Phase 9: Safe Feature Development

Feature request:\
[DESCRIBE THE FEATURE]

Before modifying code, analyze the existing implementation and reverse-engineering documentation.

Identify:

- Relevant execution flows and business rules
- Existing behavior that must be preserved
- Affected components and dependencies
- Database and schema impacts
- API and event contract changes
- Security and authorization implications
- Transaction, concurrency, and failure-handling concerns
- Required tests
- Expected files to change
- Deployment and rollback considerations

Provide an evidence-based change impact analysis and the smallest safe implementation plan.

Wait for approval before modifying code.

After approval:

1. Implement only the agreed scope.
2. Add or update tests.
3. Execute relevant tests and build.
4. Review the complete diff.
5. Check for unintended behavior changes.
6. Update affected documentation.
7. Report remaining risks and unverified assumptions.

Do not refactor unrelated code or perform a broad rewrite as part of a small feature request.

### معیار پذیرش

تغییرات دارای تحلیل اثر، تست مناسب و مستندات به‌روز هستند.

## ۱۳. مرحله دهم: نقشه راه مدرن‌سازی

### هدف

تعیین بخش‌هایی که باید حفظ، اصلاح، جداسازی یا جایگزین شوند.

### اقدامات

برای هر بخش، موارد زیر را بررسی کن:

- اهمیت کسب‌وکار
- هزینه نگهداری
- میزان تغییرات
- انسجام مسئولیت‌ها
- وابستگی‌ها
- مالکیت داده
- قابلیت تست
- ریسک جایگزینی
- پیچیدگی مهاجرت
- وابستگی سایر اجزا

هر بخش را در یکی از دسته‌های زیر قرار بده:

- حفظ و مستندسازی
- اصلاح داخلی
- ایجاد مرز ماژولی روشن
- استخراج پشت یک قرارداد مشخص
- جایگزینی تدریجی
- بررسی بازنویسی کامل

### پرامپت

# Phase 10: Modernization Roadmap

Use verified architecture findings, business flows, risks, and test results to prepare an incremental modernization roadmap.

For each candidate component, evaluate:

- Business importance
- Maintenance cost
- Change frequency
- Coupling and cohesion
- Data ownership
- Reliability and security risks
- Testability
- Replacement cost and risk
- Migration complexity
- Dependencies on other components

Classify each candidate as:

- Keep and document
- Improve internally
- Establish clearer module boundaries
- Extract behind an explicit contract
- Replace incrementally
- Consider a complete rewrite

For each proposed action, document the evidence, expected benefits, alternatives, prerequisites, implementation sequence, risks, verification method, and rollback strategy.

Do not recommend microservices or a complete rewrite solely because the existing code is poorly structured.

Create:

- `docs/reverse-engineering/10-modernization-roadmap.md`
- `docs/reverse-engineering/10-prioritized-backlog.md`

Separate confirmed problems from assumptions and improvement opportunities.

### معیار پذیرش

نقشه راه بر اساس شواهد، ریسک و اولویت کسب‌وکار تدوین شده است.

## ۱۴. قالب ثبت یافته‌ها

برای یافته‌های مهم از قالب زیر استفاده کن:

```
# Finding: [Title]

- ID: FIND-001
- Status: Observed / Runtime-Verified / Inferred / Unknown
- Confidence: High / Medium / Low
- Affected Capability: [Capability]
- Affected Components: [Components]

## Evidence
- File: [Path]
- Symbol: [Class / Function / Configuration Key]

## Observation
[What was actually found]

## Interpretation
[What the evidence suggests]

## Risk or Impact
[Potential consequences]

## Verification Needed
[Additional evidence required]

## Recommended Action
[Next step]

## Related Tests
[Test references]

## Last Verified
[Date or commit reference]
```

## ۱۵. معیارهای پایان مهندسی معکوس

- فناوری‌ها و روش اجرای پروژه مشخص شده‌اند.
- نقاط ورود و قراردادهای اصلی شناسایی شده‌اند.
- قابلیت‌ها و جریان‌های حیاتی مستند شده‌اند.
- مدل داده و وابستگی‌های مهم مشخص شده‌اند.
- معماری مشاهده‌شده از معماری استنباط‌شده تفکیک شده است.
- قواعد کسب‌وکار و ریسک‌های مهم ثبت شده‌اند.
- وضعیت تست‌های مسیرهای حیاتی مشخص است.
- مستندات با سورس اعتبارسنجی شده‌اند.
- فرایند تحلیل اثر تغییر تعریف شده است.
- نقشه راه مدرن‌سازی تهیه شده است.

# برای تولید خروچی خا به زبان فارسی
بالای پرامپتها این عبارت را درج کنید تا خروجی فارسی تولید شود

Language and Output Requirements

Write all documentation, findings, summaries, analysis, risks, and unresolved questions in clear, technically accurate Persian (Farsi).

Preserve the original English names of programming languages, frameworks, libraries, classes, methods, variables, configuration keys, file paths, commands, and technical identifiers.

Keep the documentation filename and directory structure exactly as specified in this prompt.

Use Markdown headings, lists, and tables where appropriate. Technical terms may be written in Persian followed by their English equivalents on first use.

Do not translate or modify source code, identifiers, or configuration values.

These language requirements apply to every Markdown file created or updated during this phase.