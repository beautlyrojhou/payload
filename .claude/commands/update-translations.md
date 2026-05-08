# Update Translations

Update or add translations for Payload CMS across all supported locales.

## Usage

```
/update-translations [locale] [key] [value]
```

## Arguments

- `locale` (optional): The locale code to update (e.g., `de`, `fr`, `es`). If omitted, runs interactively.
- `key` (optional): The translation key path (e.g., `general.save`, `fields.required`).
- `value` (optional): The translated string value.

## What This Command Does

1. **Identifies missing translations** by comparing against the `en` locale as the source of truth
2. **Validates existing translations** for correctness and completeness
3. **Generates new translation entries** using the generate-translations skill
4. **Formats output** consistently with existing translation files

## Steps

### 1. Locate Translation Files

Find all translation files in the project:

```bash
find . -path '*/translations/*.ts' -not -path '*/node_modules/*' | sort
```

Translation files are typically located at:
- `packages/translations/src/languages/`
- `packages/ui/src/translations/`

### 2. Identify Missing Keys

Use the `en` locale as the canonical source:

```bash
# List all keys in English locale
grep -E '^\s+[a-zA-Z]' packages/translations/src/languages/en.ts | head -50
```

Compare against target locale to find gaps.

### 3. Apply the Generate Translations Skill

Reference: `.claude/skills/generate-translations/SKILL.md`

Follow the skill instructions to:
- Extract missing keys
- Generate contextually appropriate translations
- Maintain consistent tone and terminology

### 4. Validate Translation Format

Ensure the translation file:
- Exports a default object matching the `DefaultTranslationsObject` type
- Has no TypeScript errors
- Maintains alphabetical key ordering within sections
- Uses proper pluralization patterns where needed

```typescript
// Example valid translation entry
export default {
  general: {
    cancel: 'Abbrechen',
    confirm: 'Bestätigen',
    save: 'Speichern',
  },
} satisfies DefaultTranslationsObject
```

### 5. Run Type Check

```bash
pnpm tsc --noEmit -p packages/translations/tsconfig.json
```

### 6. Update Index File

If adding a **new** locale, update the translations index:

```bash
# Check current exports
cat packages/translations/src/index.ts
```

Add the new locale export following the existing pattern.

## Supported Locales

Currently supported locales in Payload:

| Code | Language |
|------|----------|
| `en` | English (source) |
| `de` | German |
| `es` | Spanish |
| `fr` | French |
| `it` | Italian |
| `ja` | Japanese |
| `nb` | Norwegian Bokmål |
| `nl` | Dutch |
| `pl` | Polish |
| `pt` | Portuguese |
| `ro` | Romanian |
| `rs` | Serbian |
| `rsLatin` | Serbian (Latin) |
| `sv` | Swedish |
| `tr` | Turkish |
| `uk` | Ukrainian |
| `zh` | Chinese (Simplified) |
| `zhTW` | Chinese (Traditional) |

## Notes

- Never modify the `en` locale — it is the source of truth
- Preserve HTML tags and interpolation variables like `{{variable}}` exactly
- When unsure about a translation, add a `// TODO: verify` comment
- Run `pnpm lint` after changes to catch formatting issues
