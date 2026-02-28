## Major Feature

:::info
Query data inside JSON/JSONB columns as if they were native properties.
:::

### JSON Column Queries

You can now query semi-structured data stored in JSON columns directly from the ORM's fluent API. This feature enables:

- **Filter by JSON properties** - Find records based on values inside JSON
- **Access nested properties** - Navigate complex JSON structures with dot notation
- **Check key existence** - Use `.IsNull` to find records missing a property
- **Automatic type conversion** - Framework generates appropriate SQL casts automatically

**Complete Example:**

```pascal
type
  [Table('UserSettings')]
  TUserSetting = class
  public
    [PK, AutoInc]
    property Id: Integer read FId write FId;
    
    [JsonColumn]  // Mark the column as JSON
    property Preferences: string read FPreferences write FPreferences;
  end;

// Fluent JSON queries
var Users := Context.UserSettings
  .Where(Prop('Preferences').Json('theme') = 'dark')
  .Where(Prop('Preferences').Json('notifications.email') = True)
  .ToList;

// Verify non-existent keys
var NoProfile := Context.UserSettings
  .Where(Prop('Preferences').Json('profile').IsNull)
  .ToList;
```
### Multi-Database Support

| Database | JSON Function | Column Type | Status |
|----------|--------------|-------------|--------|
| PostgreSQL | `#>>` operator | `JSONB` / `JSON` | ✅ Full Support |
| SQLite 3.9+ | `json_extract()` | `TEXT` | ✅ Full Support |
| MySQL 5.7+ | `JSON_EXTRACT()` | `JSON` | ✅ Full Support |
| SQL Server 2016+ | `JSON_VALUE()` | `NVARCHAR(MAX)` | ✅ Full Support |

### Smart Type Casting

Dext automatically generates correct SQL casts:

- **INSERT PostgreSQL**: `::jsonb` applied automatically on `[JsonColumn]` columns  
- **Numeric comparisons**: `::text` applied to compare JSON text with numbers
- **NULL checking**: Correctly generates `IS NULL` for JSON expressions

### SQLite JSON Support

New compilation directive to enable JSON functions in SQLite:

```pascal
// In Dext.inc
{$DEFINE DEXT_ENABLE_SQLITE_JSON}  // Requires sqlite3.dll 3.9+ with JSON1
```

:::tip
SQLite 3.51.2+ already includes JSON support by default. Download at [sqlite.org](https://sqlite.org/download.html).
:::

### Documentation

See the Dext Book: [JSON Queries Guide](docs/Book/05-orm/json-queries.md)
