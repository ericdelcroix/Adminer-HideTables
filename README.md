# Tables Hide Plugin - Documentation

## Overview

The **AdminerTablesHide** plugin extends Adminer to permanently hide specific tables from both the sidebar menu and the main content area. Hidden tables can be selectively shown via a UI toggle, and user customizations are persisted in browser localStorage.

## Architecture

### Three-Layer System

1. **Configuration File** (`tables-hide-config.php`)
   - Declares which tables should be hidden per database
   - Supports two modes: global list or per-database lists

2. **Plugin Class** (`plugins/tables-hide.php` → `AdminerTablesHide`)
   - Reads configuration at instantiation
   - Injects JavaScript into the sidebar menu to hide/filter tables
   - Hides table rows in the main content area via DOM manipulation

3. **Entry Point** (`index.php`)
   - Loads the config file
   - Instantiates the plugin with the config array
   - Registers plugin in the Adminer plugin system

---

## Configuration

### File: `tables-hide-config.php`

Located in the Adminer root directory, this file returns a PHP array defining hidden tables.

#### Global Mode (Single List)

All tables in this list are hidden across all databases:

```php
<?php
return [
    'table_a',
    'table_b',
    'table_c',
];
```

#### Per-Database Mode (Recommended)

Tables are hidden only within their respective databases:

```php
<?php
return [
    'base_a' => [
        'specific-a',
        'specific-b',
        'specific-c',
    ],
    'base_b' => [
        'temp_import',
        'backup_2025',
    ],
];
```

#### Mixed Mode

Both global and per-database lists can coexist (global lists are fallback if current database has no per-database entry):

```php
<?php
return [
    'global_hidden_1',  // Global
    'global_hidden_2',  // Global
    'database_x' => [
        'db_specific_1',
        'db_specific_2',
    ],
];
```
