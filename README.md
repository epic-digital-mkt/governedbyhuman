# EPIC Digital · Site Brutalist Editorial

2 paginas HTML standalone para o GTM BR Nation (Abr 2026).

## Arquivos

| Arquivo | URL-alvo | Funcao |
|---------|----------|--------|
| `index.html` | `governedbyhuman.com` | Manifesto publico. Linkable no LinkedIn/DM/email. |
| `deck.html` | `deck.governedbyhuman.com` ou `revenueintelligence.app/deck` | Deck de 10 slides navegavel. Para mostrar no celular/laptop no evento. |

## Design system (committed)

- **Paleta:** Off-white `#FAFAF7` · Ink `#0A0A0A` · EPIC Red `#FF0050`
- **Display:** Fraunces (variable: opsz + SOFT + WONK — editorial com personalidade Brody-style)
- **Body:** Source Serif 4 (variable)
- **Mono:** JetBrains Mono (metadata, callouts, numbers)
- **Texture:** grain overlay SVG (opacity 0.05-0.06, mix-blend: multiply)
- **Motion:** scroll-triggered typography reveals via IntersectionObserver (manifesto); CSS transitions slide-to-slide (deck)

## Deploy em <10 min

### Opcao A — Vercel (mais rapido)

```bash
cd /Users/fabiomunhoz/epic/epic-interno/jupiter/_compendio/site
npx vercel --yes --prod
# copia a URL gerada
```

Depois no dashboard Vercel:
1. Settings → Domains → add `governedbyhuman.com`
2. Copiar os DNS targets (A record ou CNAME)
3. No GoDaddy, ajustar os registros DNS (ver secao DNS abaixo)

### Opcao B — Quave Cloud

```bash
cd /Users/fabiomunhoz/epic/epic-interno/jupiter/_compendio/site
# Apontar dominio existente. Ver /epic/team-skills/deploy-quave ou skill deploy-quave
```

### Opcao C — Netlify drag-and-drop

1. https://app.netlify.com/drop
2. Arrastar a pasta `site/` inteira
3. Atribuir dominio

### DNS (apos dominio comprado no GoDaddy)

Com Vercel (recomendado):

| Tipo | Nome | Valor |
|------|------|-------|
| A | @ | 76.76.21.21 |
| CNAME | www | cname.vercel-dns.com |

Api GoDaddy (rapido, via `~/.secrets/epic.env`):

```bash
set -a && source ~/.secrets/epic.env && set +a
curl -X PUT "https://api.godaddy.com/v1/domains/governedbyhuman.com/records/A/@" \
  -H "Authorization: sso-key ${GODADDY_API_KEY}:${GODADDY_API_SECRET}" \
  -H "Content-Type: application/json" \
  -d '[{"data":"76.76.21.21","ttl":600}]'

curl -X PUT "https://api.godaddy.com/v1/domains/governedbyhuman.com/records/CNAME/www" \
  -H "Authorization: sso-key ${GODADDY_API_KEY}:${GODADDY_API_SECRET}" \
  -H "Content-Type: application/json" \
  -d '[{"data":"cname.vercel-dns.com","ttl":600}]'
```

## Verificacao local

```bash
cd /Users/fabiomunhoz/epic/epic-interno/jupiter/_compendio/site
python3 -m http.server 8719
# abrir http://localhost:8719/index.html
# abrir http://localhost:8719/deck.html
```

## Navegacao do deck

- `→` / `Space` / `PageDown` — proximo slide
- `←` / `PageUp` — anterior
- `1-9` — ir direto para slide N
- `0` — slide 10
- `Home` / `End` — primeiro / ultimo
- Toque na metade direita / esquerda da tela — proximo / anterior
- Swipe horizontal — proximo / anterior
- Deep-link: `deck.html#slide-5` abre no slide 5

## Proximas melhorias (pos-evento)

- [ ] Open Graph image PNG (1200×630) para preview no LinkedIn/WhatsApp
- [ ] Analytics minimo (Plausible ou Fathom)
- [ ] Formulario de captura de email na secao chamado (Passo 2)
- [ ] Traducao EN simultanea com toggle de idioma
- [ ] Export do deck para PDF (via `window.print()` com @media print ja parcialmente estilizado)
- [ ] Versao audio do manifesto (Fabio ou Jeferson narrando)

## Edicao rapida durante o evento

Todos os textos estao inline no HTML. Abrir `index.html` / `deck.html` em VSCode e editar direto. Push to Vercel via `vercel --prod` para atualizar a versao publicada.

URLs de texto que aparecem em ambas as paginas:
- `revenueintelligence.app` (home produto)
- `governedbyhuman.com` (manifesto)
- `categoryleap.com` (framework)
- `epic.digital` (casa)

## Fonts notice

As fontes vem via Google Fonts CDN. Funciona offline apenas nas primeiras 7 dias apos visita devido a service worker cache do navegador. Para offline garantido, self-host Fraunces + Source Serif 4 + JetBrains Mono (adicionar `@font-face` no `<style>`).
