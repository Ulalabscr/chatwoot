# Branding Ulábot — archivos tocados sobre upstream

Parche de marca sobre `ula/v4.17.0`. Solo se cambian **valores**, nunca claves ni placeholders (`{latestChatwootVersion}` etc.).
No tocar `enterprise/` (licencia propietaria; el build CE la borra). No borrar `LICENSE`.

## Qué se cambió
- `config/installation_config.yml`: valores por defecto de INSTALLATION_NAME, BRAND_NAME, BRAND_URL, WIDGET_BRAND_URL, TERMS_URL, PRIVACY_URL (solo aplican a instalaciones nuevas; en la DB actual se actualizan por consola).
- `public/manifest.json`: `name` y `short_name`.
- `app/views/layouts/mailer/base.liquid`, `app/views/devise/mailer/confirmation_instructions.html.erb`: marca de respaldo.
- `app/views/mailers/administrator_notifications/**`: 3 plantillas de borrado de cuenta.
- `app/javascript/dashboard/i18n/locale/{es,en}/`: login, resetPassword, settings, generalSettings, onboarding, conversation, inboxMgmt, integrations, signup.

## Logos e íconos
- `public/brand-assets/logo.svg` (claro, sin eslogan), `logo_dark.svg` (neón sobre transparente), `logo_thumbnail.svg` (cabeza del mono): son PNG embebidos en SVG (no vectores), máx. 640 px de ancho; mismos nombres que upstream.
- `public/{favicon,favicon-badge,android-icon,apple-icon,apple-touch-icon,ms-icon}*.png`: cabeza del mono, mismos tamaños que upstream.
- `public/manifest.json` y meta de `layouts/vueapp.html.erb`: colores `#2781F6` -> `#000000`.

## Rendimiento (ula.4)
- `app/javascript/dashboard/i18n/index.js`: solo se empaquetan `en`, `es`, `pt`, `pt_BR` (upstream importa 57 idiomas = 31 MB de JSON; el chunk compartido pasaba de 14,8 MB / 4,1 MB gzip). `fallbackLocale: 'en'` en `entrypoints/dashboard.js` y `v3app.js`. Tras un merge de upstream, este archivo puede dar conflicto: conservar la lista corta.
- Logos SVG reducidos (420 px / 128 px, PNG cuantizado) y PNG de íconos cuantizados; los `<link>` de íconos en `vueapp.html.erb` llevan `?v=ula4` (subir el valor al cambiar íconos).

## Embed en el panel + push con texto (ula.5)
- `app/controllers/dashboard_controller.rb`: `after_action :allow_panel_embedding` quita `X-Frame-Options` y pone `Content-Security-Policy: frame-ancestors 'self' https://panel.ulalabscr.com` (configurable con `PANEL_FRAME_ANCESTOR`). Solo `/app/*` (dashboard y login); el resto de controladores no cambia.
- `app/services/notification/push_notification_service.rb`: `push_message` agrega `body: notification.push_message_body` al payload del push de navegador.
- El redirect tras SSO ya existe en upstream (`sso_account_id` / `sso_conversation_id` en `/app/login`), no se parchea.
- Upstream puede chocar en estos dos archivos al hacer merge: reaplicar a mano.

## Pendiente
- Archivos i18n poco visibles sin tocar: mfa, labelsMgmt, auditLogs, yearInReview, helpCenter, generalSettings (resto).

## Revisar tras cada merge de upstream
```
grep -rn "Chatwoot" app/javascript/dashboard/i18n/locale/{es,en}/{login,resetPassword,settings,generalSettings,onboarding,conversation,inboxMgmt,integrations,signup}.json
```
