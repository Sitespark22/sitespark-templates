# SiteSpark Templates — Stage 2

20 HTML landing page templates for the SiteSpark automated build pipeline. Each template is a standalone single-page site with `{{SLOT_NAME}}` placeholders that get filled by Claude before Netlify deploy.

## Template Library

| Folder | Industry |
|--------|----------|
| `auto-repair` | Auto Repair & Detailing |
| `barbershop` | Barbershop |
| `cleaning-services` | Cleaning Services |
| `coach-consultant` | Coach / Consultant |
| `cpa-financial` | CPA & Financial Services |
| `dentist-medical` | Dentist & Medical |
| `event-venue` | Event Venue |
| `fitness-gym` | Fitness & Gym |
| `general-service` | General / Other |
| `hair-salon` | Salon & Beauty |
| `home-services` | Home Services (Plumbing, HVAC, Electric) |
| `landscaping` | Landscaping & Lawn Care |
| `law-firm` | Legal & Professional Services |
| `med-spa` | Med Spa |
| `photographer` | Photography & Creative |
| `real-estate` | Real Estate |
| `restaurant-cafe` | Restaurant & Food Service |
| `retail-boutique` | Retail & E-commerce |
| `trades-contractor` | Roofing, Painting, Construction |
| `veterinarian` | Pet Services / Vet |

## Universal Slots (all 20 templates)

| Slot | Source Field |
|------|-------------|
| `{{BUSINESS_NAME}}` | `businessName` |
| `{{TAGLINE}}` | `tagline` |
| `{{HERO_HEADLINE}}` | `tagline` or generated |
| `{{HERO_SUBTEXT}}` | Generated from address/city |
| `{{COLOR_PRIMARY}}` | `primaryColor` |
| `{{COLOR_SECONDARY}}` | `secondaryColor` |
| `{{COLOR_BG}}` | `accentColor` |
| `{{PHONE}}` | `clientPhone` |
| `{{EMAIL}}` | `clientEmail` |
| `{{ADDRESS}}` | `address` |
| `{{CTA_TEXT}}` | `ctaText` |
| `{{CTA_LINK}}` | `ctaLink` |
| `{{BOOKING_URL}}` | `ctaLink` |
| `{{META_TITLE}}` | `seoTitle` |
| `{{META_DESCRIPTION}}` | `tagline` |
| `{{LOGO_URL}}` | (empty — uses fallback text) |
| `{{SERVICES_LIST}}` | `services` (newline-separated → `<li>` items) |

## How It Works in n8n

1. **Intake form** (`admin.sitesparkco.org`) collects `businessType` + brand data
2. **Map Business Type node** converts `businessType` to a folder slug
3. **Fetch Template node** — `GET https://raw.githubusercontent.com/Sitespark22/sitespark-templates/main/{slug}/index.html`
4. **Build Claude Prompt node** sends template + form data to Claude with slot-fill instructions
5. **Claude** returns completed HTML (no markdown, just the file)
6. **Netlify deploy** — file goes live as `index.html`

## Business Type → Folder Mapping

```
Home Services (Plumbing, HVAC, Electric) → home-services
Landscaping & Lawn Care                   → landscaping
Roofing & Gutters                         → trades-contractor
Painting (Interior / Exterior)            → trades-contractor
Cleaning Services                         → cleaning-services
Auto Repair & Detailing                   → auto-repair
Restaurant & Food Service                 → restaurant-cafe
Healthcare & Medical                      → dentist-medical
Legal & Professional Services             → law-firm
Real Estate                               → real-estate
Salon & Beauty                            → hair-salon
Fitness & Wellness                        → fitness-gym
Retail & E-commerce                       → retail-boutique
Construction & Contracting                → trades-contractor
Pet Services                              → veterinarian
Photography & Creative                    → photographer
Other                                     → general-service
```

## Adding a New Template

1. Create a new folder with the slug name
2. Add `index.html` with all universal slots + any template-specific slots
3. Add the new business type → slug mapping to the n8n "Map Business Type" Code node
4. Add the new option to the `lp-businessType` dropdown in the intake dashboard
