## Major Features

The final evolution of Dext ORM and Web API before v1.0.

### ORM Evolution & Fluency

Dramatic simplification of data exposure and complex query execution.
- **MapDataApi<T>** - New fluent syntax to create full REST endpoints from an entity with a single line of code.
- **FromSql Support** - Execute raw SQL directly via `DbContext.Users.FromSql(...)` while maintaining automatic object mapping.
- **Multi-Mapping ([Nested])** - Dapper-style recursive hydration support. Map complex objects in a single query using the `[Nested]` attribute.
- **Pessimistic Locking** - Full concurrency control with native support for `FOR UPDATE` (PostgreSQL/Oracle) and `UPDLOCK` (SQL Server).
- **Stored Procedures Evolution** - Declarative mapping via `[StoredProcedure]` and `[DbParam]` attributes for input and output parameters.

### Web & Performance

Performance and flexibility in data filtering.
- **Zero-Allocation JSON** - "Database as API" engine now uses `TUtf8JsonWriter` for direct streaming from database to socket, minimizing memory allocations.
- **Dynamic Specification Mapping** - Integrated advanced QueryString filtering (`_gt`, `_lt`, `_sort`, etc) that automatically maps to SQL.
- **Core Interception** - The Proxy and ClassProxy engine has been moved to Core, eliminating circular dependencies and optimizing Lazy Loading.

### New Examples

- **eShopOnWeb**: Complete implementation of Microsoft's classic demo adapted for Dext.
- **HelpDesk**: Ticketing system with layered architecture and integration testing.
- **MultiTenancy**: Data isolation demonstration by schema and by database.
- **SmartPropsDemo**: Advanced use of `Prop<T>` and `Nullable<T>` with persistence.

### Bug Fixes & Stability

- **SQL Generator**: Improvement in Foreign Keys generation, ignoring navigation properties during `CREATE TABLE`.
- **Memory Management**: Resolved ownership conflicts in the `THandlerInvoker` and memory leaks in data seeding.
- **Lazy Loading**: Correction of Access Violations caused by incorrect proxy initialization.
- **TActivator**: Smart prioritization of derived class builders.
