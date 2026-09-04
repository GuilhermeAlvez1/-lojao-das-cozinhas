# HOME_REDESIGN_PLAN.md
Lojão das Cozinhas — Plano de redesign da HOME (Fase 0 · Auditoria)
Data: 24/08/2026 · Nenhuma alteração de código realizada nesta fase.

---

## A. ESTADO ATUAL

### Stack
- HTML estático puro (10 páginas), sem framework, sem build system, sem bundler.
- CSS único: `assets/style.css` (365 linhas) com design tokens em CSS custom properties.
- JS vanilla: `assets/main.js` (menu, ano dinâmico, formulário→WhatsApp) e `assets/banner.js` (carrossel de clientes com auto-scroll + drag).
- Zero dependências externas de CSS/JS, exceto o script Trustindex (avaliações Google).
- Sem git no diretório de trabalho, sem lint, sem testes, sem analytics.

### Arquivos da HOME
- `index.html` (298 linhas)
- `assets/style.css`
- `assets/main.js`
- `assets/banner.js`
- `assets/hero-video.mp4` (2,5 MB)
- `assets/img/*.png` (9 fotos de produto + 2 logos) e `assets/img/clientes/*.png` (10 logos)

### Seções atuais da HOME (ordem real)
1. Header (logo + nav com dropdown + CTA WhatsApp)
2. Hero (texto + vídeo da caldeira em autoplay/loop)
3. Clientes ("Quem confia…", carrossel com 10 logos, duplicados para loop)
4. Equipamentos (grid 9 cards, todos com foto real, altura fixa 260px, contain)
5. Segmentos (7 ícones, todos linkando para contato.html)
6. Cotação rápida (formulário e-mail/tel/CNPJ/arquivo → abre WhatsApp)
7. Stats (+15 anos · 2.000m² · +500 projetos · 100% sob medida)
8. Avaliações Google (script Trustindex)
9. CTA final
10. Footer (4 colunas) + WhatsApp flutuante + barra mobile fixa

### Design system existente (a preservar)
- Tokens: `--orange #EC5E36`, `--navy #1D3655`, `--cream #F6F2EA`, escala de espaçamento 8px (`--s1…--s12`), `--radius`, `--shadow`, `--touch-min:44px`.
- Fontes de sistema (zero webfonts) — bom para performance.
- Breakpoints: 600px, 900/980px (grid e menu).
- CTAs: `.btn-primary` (laranja) e `.btn-ghost` (outline).

### Funcionalidades existentes
- Menu mobile com `aria-expanded`.
- Formulários geram mensagem de WhatsApp com `encodeURIComponent` (correto).
- Carrossel de clientes com drag e loop infinito via `requestAnimationFrame`.
- Ano do footer dinâmico via `[data-year]`.
- Schema.org LocalBusiness (com dados placeholder — problema, ver C).

---

## B. MAPA DE ALTERAÇÃO POR SEÇÃO

| # | Seção | Situação atual | Ação | Arquivos |
|---|-------|----------------|------|----------|
| 1 | Header | Nav: Home/Institucional▾/Produtos/Informações/Blog/Contato + CTA "WhatsApp" | ALTERAR: renomear CTA p/ "Solicitar orçamento"; avaliar itens Soluções/Projetos/Segmentos como âncoras (sem criar páginas falsas) | index.html + demais páginas (header replicado) |
| 2 | Hero | Foco em produto único (vídeo caldeira); headline "Cozinhas industriais em aço inox, fabricadas sob medida"; selo cita "garantia" (não confirmada) | ALTERAR: nova copy "Sua cozinha profissional, do projeto à operação"; CTAs "Enviar meu projeto"/"Falar com especialista"; microprova Projeto•Fabricação•Equipamentos•Instalação•Assistência; manter vídeo provisoriamente (sem mídia melhor disponível) | index.html, style.css |
| 3 | Prova de confiança | Carrossel ok, 10 logos reais; velocidade 0.8px/frame; sem prefers-reduced-motion | ALTERAR: reduzir velocidade, pausar no hover, respeitar reduced-motion; headline "Empresas que já confiaram…" | banner.js, index.html |
| 4 | Solução completa (cadeia de valor) | NÃO EXISTE | CRIAR: seção "Uma empresa para toda a cozinha" com 5 etapas em linha (Projeto→Fabricação→Fornecimento→Instalação→Assistência) | index.html, style.css |
| 5 | Case em destaque | NÃO EXISTE | CRIAR estrutura com estado provisório claramente marcado (`data-pending="case"`); não publicar case falso | index.html, style.css |
| 6 | Segmentos | 7 ícones decorativos, todos → contato.html | ALTERAR: adicionar 1 linha de particularidade por segmento; links preparados p/ páginas futuras (por ora âncora/contato com mensagem WhatsApp contextual) | index.html |
| 7 | Linhas de equipamentos | Grid 9 cards com fotos ok | MANTER estrutura; ALTERAR taxonomia dos nomes (Cocção, Caldeirões, Mobiliário inox, Pias e higienização, Refrigeração, Exaustão, Distribuição, Cafeteiras) — validar nomes com o dono antes | index.html |
| 8 | Fabricação sob medida | Só existe como card em Serviços; página interna "em construção" | CRIAR seção na home "Seu espaço não precisa se adaptar ao catálogo" + CTA "Enviar medidas ou projeto" | index.html |
| 9 | Processo de trabalho | REMOVIDO em iteração anterior a pedido | RECRIAR em formato jornada do cliente (6 passos), diferente da seção 4 | index.html |
| 10 | Projetos realizados | NÃO EXISTE | CRIAR estrutura vazia/provisória; sem conteúdo real, manter oculta (feature-flag por classe) até receber fotos | index.html, style.css |
| 11 | Fábrica/bastidores | NÃO EXISTE | CRIAR estrutura preparada; oculta até receber fotos reais | index.html |
| 12 | Área para projetistas | NÃO EXISTE | CRIAR seção com CTA "Acessar biblioteca técnica" desabilitado/em breve (sem downloads falsos) | index.html |
| 13 | Assistência técnica | Diluída em Serviços | CRIAR seção própria "Nossa responsabilidade não termina na entrega" + CTA distinto "Solicitar assistência técnica" (mensagem WhatsApp própria) | index.html, main.js |
| 14 | Avaliações | Script Trustindex com ID aparentemente demonstrativo | INVESTIGAR: validar ID real com o dono; se inválido, remover temporariamente (sem mascarar) | index.html |
| 15 | Upload planta/projeto/lista | Formulário atual pede CNPJ obrigatório visualmente; sem estados de loading/sucesso/erro; upload só "carona" no WhatsApp | ALTERAR: campos nome/empresa/e-mail/tel/segmento/cidade/mensagem/arquivo; CNPJ opcional; estados de feedback; deixar claro que o arquivo é anexado na conversa do WhatsApp (não há backend) | index.html, main.js |
| 16 | Stats | 4 números INVENTADOS | REMOVER da interface pública; deixar bloco comentado com `TODO: dados pendentes de validação` | index.html |
| 17 | CTA final | OK | MANTER com copy ajustada | index.html |
| 18 | Footer | Telefones/endereço/CNPJ placeholder | ALTERAR: remover placeholders; exibir apenas o que for confirmado | todas as páginas |

---

## C. PROBLEMAS ENCONTRADOS (por criticidade)

### Críticos (dados fictícios públicos)
1. **108 ocorrências de placeholders** nos 10 HTML: WhatsApp `5511900000000`, telefone `551130000000`, "Rua Exemplo, 000", CNPJ `00.000.000/0001-00`, CEP `00000-000`.
2. **Stats inventados** na home: +15 anos, 2.000m², +500 projetos, 100% sob medida.
3. **Schema.org LocalBusiness com endereço/telefone falsos** — pior que não ter: ensina dado errado ao Google.
4. **Selo do hero promete "garantia"** — claim não confirmado.
5. **Trustindex com ID não validado** — risco de seção de avaliações vazia/quebrada.
6. **Título/descrição SEO afirmam "Fábrica de Equipamentos"** — a missão exige diferencial fabricamos/fornecemos/instalamos/mantemos, sem claim de que tudo é fabricado internamente.

### Estruturais
7. Home não comunica cadeia de valor (projeto→operação); hero vende 1 produto.
8. 5 páginas internas com "Conteúdo em construção" público (produtos, blog, informações, fabricação-sob-medida, fogões).
9. Segmentos são só ícones sem conteúdo, todos apontando ao mesmo lugar.
10. Não existe caminho próprio para assistência técnica.

### Técnicos
11. `prefers-reduced-motion` não é respeitado (carrossel roda sempre).
12. Fotos de produto sem `width`/`height` → risco de CLS.
13. PNGs pesados (fogao 552KB, cafeteira 520KB, logo-redondo 2MB!) sem WebP/srcset.
14. Vídeo 2,5MB carrega em todas as conexões, sem `poster` nem `preload` controlado.
15. Header/footer duplicados manualmente em 10 arquivos (manutenção frágil — sem framework, é limitação aceitável, mas cada mudança exige replicação).
16. Menu mobile não bloqueia scroll de fundo quando aberto.
17. Focus-visible parcial nos cards.

---

## D. O QUE SERÁ PRESERVADO
- Stack HTML/CSS/JS puro — nenhum framework será adicionado (§40).
- Todos os design tokens (`--orange`, `--navy`, `--cream`, espaçamento 8px, radius, sombras).
- Tipografia de sistema (zero webfonts).
- Grid de equipamentos com as 9 fotos reais já padronizadas (contain, 260px, fundo branco).
- Carrossel de clientes com os 10 logos reais (só ajustado, não recriado).
- Formulário→WhatsApp com `encodeURIComponent` (padrão correto já usado).
- Barra mobile fixa e botão WhatsApp flutuante.
- Estrutura semântica existente (header/nav/section/footer, aria básico do menu).
- Todas as páginas internas (nenhuma será excluída).

## Dependências
Nenhuma nova dependência será instalada. Tudo é implementável com o HTML/CSS/JS existente. Trustindex permanece como único terceiro, condicionado à validação do ID.

---

## E. O QUE SERÁ ALTERADO (resumo)
- Copy e estrutura do hero (posicionamento "do projeto à operação").
- Remoção de todos os dados fictícios públicos (placeholders ficam como comentários `TODO-DADO-PENDENTE` no código).
- Schema.org reduzido ao que é verdadeiro (nome, url, descrição) até receber dados reais.
- Nova arquitetura de seções conforme mapa B (criações com estado provisório onde faltar conteúdo real, jamais conteúdo inventado).
- Acessibilidade: reduced-motion, foco visível, scroll-lock do menu, width/height nas imagens.
- Performance: conversão das fotos para WebP com fallback, compressão do logo 2MB, poster no vídeo.
- WhatsApp com mensagens contextuais por seção.

---

## F. PLANO POR FASES

- **FASE 0 — Auditoria** ✅ (este documento)
- **FASE 1 — Fundação**: remover dados fictícios (HTML + schema), corrigir claim de garantia, tokens ok (já existem), width/height nas imagens, reduced-motion, scroll-lock do menu. Validar.
- **FASE 2 — Posicionamento**: novo hero (copy + CTAs + microprova), ajuste do carrossel de prova, seção "Uma empresa para toda a cozinha". Validar.
- **FASE 3 — Autoridade**: estruturas de case, fabricação sob medida, processo (jornada), projetos/fábrica ocultos até haver material real. Validar.
- **FASE 4 — Oferta**: segmentos com particularidades, taxonomia das linhas (após validação de nomes), seção de assistência técnica. Validar.
- **FASE 5 — Conversão**: formulário evoluído (campos, CNPJ opcional, estados de feedback), WhatsApp contextual, hierarquia de CTAs. Validar.
- **FASE 6 — Técnico**: SEO (titles/descriptions), structured data verdadeiro, WebP/srcset, poster do vídeo, auditoria responsiva 320→1920px.
- **FASE 7 — QA final**: navegação, links, formulários, console, acessibilidade, checklist §45.

Cada fase replica mudanças de header/footer nas 10 páginas para não divergirem.

---

## G. RISCOS
1. **Remover stats e telefone placeholder deixa a home "sem números" e sem contato clicável** até o dono fornecer dados reais → risco aceito pela missão (credibilidade > impressão).
2. **Trustindex pode ser conta demonstrativa** → seção pode precisar sair temporariamente.
3. **Header/footer duplicados em 10 arquivos** → qualquer esquecimento gera inconsistência; mitigação: script de verificação (diff) ao final de cada fase.
4. **Sem ambiente de execução com navegador real aqui** → validação visual limitada a inspeção de código + capturas do usuário; mitigação: entregar checklist de teste manual junto com cada fase.
5. **Renomear categorias de equipamentos** (taxonomia §15) pode divergir do vocabulário que o dono usa com clientes → precisa aprovação antes.
6. **Não há backend**: upload de arquivo continua dependendo do anexo manual no WhatsApp; prometer upload "real" seria mentira técnica.

---

## H. INFORMAÇÕES/MATERIAIS REAIS PENDENTES (fornecer quando possível)
1. WhatsApp comercial real (com DDD).
2. Telefone fixo real (se houver).
3. Endereço completo real + CEP.
4. CNPJ e razão social.
5. Horário de funcionamento real.
6. E-mail real.
7. Números reais (anos de mercado, área, projetos) — OU confirmação de que ficam fora do site.
8. Confirmação: a empresa dá garantia? De quê? Por quanto tempo?
9. ID real do Trustindex (ou confirmação de que não existe conta).
10. Fotos reais: fábrica/produção, equipe, ao menos 1 projeto entregue (antes/depois se possível).
11. 1 case real mínimo: cliente, segmento, o que foi feito (com autorização de uso).
12. Confirmação da taxonomia de categorias (posso propor, você aprova).
13. Definição do escopo real: o que é fabricado internamente vs. fornecido de terceiros (para o texto "fabricamos/fornecemos" ser exato).
14. Autorização de uso dos 10 logos de clientes exibidos.
