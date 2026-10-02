# Branding Ulábot — archivos tocados sobre upstream

Parche de marca sobre `ula/v4.17.0`. Solo se cambian **valores**, nunca claves ni placeholders (`{latestChatwootVersion}` etc.).
No tocar `enterprise/` (licencia propietaria; el build CE la borra). No borrar `LICENSE`.

## Qué se cambió
- `config/installation_config.yml`: valores por defecto de INSTALLATION_NAME, BRAND_NAME, BRAND_URL, WIDGET_BRAND_URL, TERMS_URL, PRIVACY_URL (solo aplican a instalaciones nuevas; en la DB actual se actualizan por consola).
- `public/manifest.json`: `name` y `short_name`.
- `app/views/layouts/mailer/base.liquid`, `app/views/devise/mailer/confirmation_instructions.html.erb`: marca de respaldo.
- `app/views/mailers/administrator_notifications/**`: 3 plantillas de borrado de cuenta.
- `app/javascript/dashboard/i18n/locale/{es,en}/`: login, resetPassword, settings, generalSettings, onboarding, conversation, inboxMgmt, integrations, signup.

## Pendiente
- Logos e íconos en `public/` (mismos nombres de archivo): brand-assets/logo*.svg, favicon-*, android-icon-*, apple-icon-*, ms-icon-*.
- Archivos i18n poco visibles sin tocar: mfa, labelsMgmt, auditLogs, yearInReview, helpCenter, generalSettings (resto).

## Revisar tras cada merge de upstream
```
grep -rn "Chatwoot" app/javascript/dashboard/i18n/locale/{es,en}/{login,resetPassword,settings,generalSettings,onboarding,conversation,inboxMgmt,integrations,signup}.json
```
