# Translation Manager

Translate entire locales in seconds using bulk AI, with file-based storage, zero runtime DB queries, codebase scanning for missing keys, and vendor package and CSV support.

![Translation Manager for Filament](https://raw.githubusercontent.com/craft-forge/filament-translation-manager-docs/master/.github/translation-manager.jpg)

---

## Highlights

### Zero Runtime Overhead
Unlike database-driven translation managers, this plugin uses **file-based storage**. Your translations are published to standard Laravel `lang/` files, meaning:
- **Zero database queries** for translations at runtime
- Full compatibility with Laravel's translation caching
- No performance impact on your production application

### AI-Powered Workflow
Translate instantly with **integrated AI services**:
- One-click translation for individual fields
- Bulk translate entire locales in seconds
- Choose between: DeepL, Google, ChatGPT, or Claude

### Laravel-Native Integration
Built specifically for Laravel's translation system:
- **Auto-generates** translation keys by scanning your code
- **Manage vendor packages** and Laravel core translations from a unified UI
- Preserves placeholders (`:attribute`, `:count`) during AI translation
- Handles pluralization syntax (`{0} None|{1} One|[2,*] :count items`)
- Supports JSON translations, PHP group files, and vendor overrides

---

## Features

|    | Feature                | Description                                                                                              |
|----|------------------------|----------------------------------------------------------------------------------------------------------|
| 🤖 | **AI Translation**     | Translate using DeepL, Google Translate, ChatGPT, or Claude — individual fields or entire locales        |
| 📁 | **File-Based Storage** | Publish to Laravel `lang/` files for native performance and caching                                      |
| 🔄 | **Two-Way Sync**       | Import from files, edit in UI, and publish back to files                                                 |
| ✏️ | **Inline Editing**     | Edit all locales for a key on a single page                                                              |
| 📦 | **Package Management** | Publish and manage vendor package translations and Laravel core files from a unified UI                  |
| 🎯 | **Smart Filters**      | Filter by group, vendor, type, date range, missing translation, duplicate values, or unpublished changes |
| 🔍 | **Code Scanner**       | Automatically find and generate translation keys from your codebase                                      |
| 🔎 | **Key Usage Finder**   | See exactly where each translation key is used in your code                                              |
| 📊 | **CSV Import/Export**  | Share translations with external translators or backup your work                                         |
| 💾 | **Automatic Backups**  | Automatic snapshots before publishing translations to files                                              |
| 📝 | **Change Tracking**    | Visual indicators show which translations differ from published lang files                               |

#### Clean Installation

No custom themes, no published views, no complex setup. Just one migration (a single `translations` table) and a config file.

---

## Installation

| Plugin Version | Filament Version | PHP Version |
|----------------|------------------|-------------|
| 1.x            | 3.x              | \>= 8.2     |
| 2.x            | 4.x / 5.x        | \>= 8.2     |

Translation Manager is distributed via [Anystack](https://checkout.anystack.sh/translation-manager). After purchase, activate your license in your Anystack account and follow the installation instructions provided there.

### Step 1: Configure Composer

Add the Anystack repository to your `composer.json`:

```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://translation-manager.composer.sh"
        }
    ]
}
```

### Step 2: Authenticate

Store your license credentials for this project:

```bash
composer config http-basic.translation-manager.composer.sh EMAIL "LICENSE_KEY:domain.com"
```

- Replace `EMAIL` with your email address
- Replace `LICENSE_KEY` with your license key
- Replace `domain.com` with your production domain (e.g., `myapp.com`)

Use the same domain for both local development and production environments.

### Step 3: Install the Package

```bash
composer require craft-forge/filament-translation-manager
```

### Step 4: Run Installation

```bash
php artisan filament-translation-manager:install
```

This will publish and run the database migration (`translations` table).

### Step 5: Publish Configuration

```bash
php artisan vendor:publish --tag="filament-translation-manager-config"
```

### Step 6: Register the Plugin

Register the plugin in your Filament Panel:

```php
use CraftForge\FilamentTranslationManager\FilamentTranslationManagerPlugin;

public function panel(Panel $panel): Panel
{
    return $panel
        ->plugins([
            FilamentTranslationManagerPlugin::make(),
        ]);
}
```

---

## Usage

### Workflow

1. **Import** existing translations from your `lang/` files
2. **Edit** translations in the Filament UI
3. **Publish** changes back to `lang/` files

<details>
<summary><strong>💡 Alternative: Gitignore Lang Directory</strong></summary>
To avoid merge conflicts in `lang/` when working in a team, exclude translations from version control:

```gitignore
# .gitignore
/lang/*
!/lang/.gitkeep
```

Translations are then managed via the UI and published to files; for deployment use CSV export/import or the Artisan commands (see below).
</details>

---

### Translation List

![Translation List](.github/list.jpg)

Browse all translations with dynamic columns for each locale. Search by key, group, or translation value. Filter by group, vendor (package), translation type (regular/vendor), date range (updated from/until), missing translation, duplicate values, or unpublished changes.

---

### Import Translations

![Import Translations](.github/import.gif)

Load translations from your `lang/` files into the database with optional code scanning to catch new keys first.

**CSV workflows:**
- **CSV Export:** Download all translations for external translators or backup
- **CSV Import:** Upload translations from a CSV file

---

### Editing Translations

![Edit Translation](.github/edit-page.jpg)

Edit all locale values for a translation key on a single page. Each locale field shows AI translation buttons (DeepL, Google, ChatGPT, Claude) when services are configured — click to translate that field instantly.

---

### Bulk AI Translation

![AI Bulk Translation](.github/ai-translate.gif)

Translate an entire locale in seconds. Click **AI Translate** from the list page:

1. Select target language
2. Choose translation service (DeepL, Google, ChatGPT, or Claude)
3. Select mode: translate only untranslated values, or retranslate everything
4. Watch real-time progress

Laravel placeholders and pluralization syntax are preserved automatically.

---

### Change Tracking

![Unpublished Changes](.github/unpublished-changes.jpg)

The plugin tracks which translations have unpublished changes — values that differ from what's in your `lang/` files.

**Visual indicators:**
- **Table view:** Modified locale values are highlighted in green
- **Edit page:** Each modified field shows the previous value from files
- **Filter:** Use "Has Unpublished Changes" filter to see only modified translations

This helps you know exactly what will be written when you click **Publish**.

> **Note:** Change tracking can be disabled in config with `'track_changes' => false`

---

### Publishing

Click **Publish** to write all translations from the database to your `lang/` files. A backup is automatically created before publishing.

---

### Package Management

![Package Management](.github/packages.jpg)

Click **Manage Packages** to open the package management panel:

- **Publish** vendor package translations to `lang/vendor/{package}/` for editing
- **Publish** Laravel core translations (`auth.php`, `validation.php`, `passwords.php`, `pagination.php`)
- **Delete** published translations when no longer needed
- View **groups** and **translations count** for each package

Published translations are automatically imported into the database and become editable in the UI. After editing, use **Publish** to write changes back to files.

---

### Key Usage Finder

![Key Usage](.github/check-usage.jpg)

Click **Check Usage** on any translation to see where it's used in your codebase. Laravel system keys (validation, auth, etc.) are recognized as framework-internal.

### Backup & Restore

![Backup Restore](.github/backups.jpg)

The plugin automatically creates a backup before **Publish** — the operation that writes translations from the database to `lang/` files. This ensures translations can be restored if needed.

Any previous backup can be restored from the **CSV → Backups** menu. Note: restoring a backup will replace all current translations in the database.

---

## Authorization

```php
FilamentTranslationManagerPlugin::make()
    ->authorize(fn ($user) => $user->can('manage-translations'))
```

---

## Configuration

```php
// config/translation-manager.php

return [
    // Supported locales (first is a source language for AI)
    'locales' => ['en', 'de', 'fr', '...'],

    // Navigation settings
    'navigation' => [
        'group' => 'Administration',
        'label' => 'Translations',
        'icon' => 'heroicon-o-language',
        'sort' => 100,
    ],

    // Protect keys from accidental edits
    'disable_key_and_group_editing' => true,

    // Directories to scan for translation keys
    'scan_directories' => [
        app_path(),
        resource_path('views'),
        // base_path('routes'),
        // base_path('modules'),
    ],

    // Automatic backups
    'backup' => [
        'enabled' => true,
        'path' => storage_path('app/translation-backups'),
        'keep_last' => 10,
    ],

    // Flag icons display next to locale names in table columns and form labels
    'flags' => [
        'show_flags' => true,
        'circular_flags' => false,
    ],

    // Show visual indicators for unpublished changes
    'track_changes' => true,

    // AI Translation Services
    // Configure the services as needed
    'translation_service' => [
    
        // DeepL - High quality, especially for European languages
        // Free plan: 500,000 chars/month
        // Get API key: https://www.deepl.com/your-account/keys
        'deepl' => [
            'enabled' => true,
            'api_key' => env('DEEPL_API_KEY'),
            // Free: api-free.deepl.com | Pro: api.deepl.com
            'api_url' => env('DEEPL_API_URL', 'https://api-free.deepl.com/v2/translate'),
        ],
        
        // Google Cloud Translation
        // Get API key: https://console.cloud.google.com/apis/credentials
        // Enable API: https://console.cloud.google.com/apis/library/translate.googleapis.com
        'google' => [
            'enabled' => true,
            'api_key' => env('GOOGLE_TRANSLATE_API_KEY'),
        ],
        
        // OpenAI (ChatGPT)
        // Get API key: https://platform.openai.com/api-keys
        'openai' => [
            'enabled' => true,
            'api_key' => env('OPENAI_API_KEY'),
            'model' => env('OPENAI_MODEL', 'gpt-5.2'),
            'temperature' => 0.3,
        ],
        
        // Anthropic Claude
        // Get API key: https://console.anthropic.com/settings/keys
        'claude' => [
            'enabled' => true,
            'api_key' => env('ANTHROPIC_API_KEY'),
            'model' => env('ANTHROPIC_MODEL', 'claude-haiku-4-5-20251001'),
        ],
    ],
];
```

---

## Artisan Commands

All these operations are available through the UI, but can also be run via command line:

```bash
# Scan code and generate missing translation keys to lang/ files
php artisan translations:generate

# Import translations from lang/ files to database
php artisan translations:import

# Publish translations from database to lang/ files
php artisan translations:publish
```

---

## Customizing Plugin Translations

Publish the plugin's language files (`lang/vendor/filament-translation-manager`) to customize UI text:

```bash
php artisan vendor:publish --tag="filament-translation-manager-translations"
```

---

## Testing

The plugin includes 600+ unit and integration tests covering all functionality.

```bash
composer test
```

---

## Support

For assistance:

- **Email:** [taraskovaldev@gmail.com](mailto:taraskovaldev@gmail.com)
- **GitHub Issues:** [Report a bug](https://github.com/craft-forge/filament-translation-manager/issues)

## License

Translation Manager is proprietary software. See [LICENSE.md](LICENSE.md) for details.
