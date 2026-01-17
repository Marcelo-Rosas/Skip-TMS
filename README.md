# SKIP TMS — Vectra Cargo (v4.2.1)

Pacote de documentação **completo** até a última alteração.

## O que mudou agora (v4.2.1)
- Corrigido erro Supabase **42P10** no `import_reference_data_v4()` (pricing_generalidades `ON CONFLICT`)
- Novo loader: `SUPABASE_REFERENCE_DATA_LOADER_V4_2_1.sql`
- Hotfix para quem já executou loader antigo: `SUPABASE_HOTFIX_V4_2_1.sql`

## Regras críticas do produto
- Embarcador obrigatório (cotação e operação)
- CIF/FOB obrigatório
- Metodologia: Lotação ou LTL
- Piso ANTT: **alerta somente Lotação + TAC**, nunca LTL
- ICMS: **informativo** (percent × freight_value), não soma no frete

## Supabase — DB first (recomendado)
### Novo projeto
1) `SUPABASE_SCHEMA_V4_2.sql`
2) `SUPABASE_SEED_V4_2.sql`
3) `SUPABASE_REFERENCE_DATA_LOADER_V4_2_1.sql`
4) `SUPABASE_REFERENCE_DATA_V4_2.sql`

### Se deu erro 42P10
1) `SUPABASE_HOTFIX_V4_2_1.sql`
2) Reexecute `SUPABASE_REFERENCE_DATA_V4_2.sql`

## Diagramas
- Mermaid: `DIAGRAMS_MERMAID_SKIP_TMS_V4_2_1.md`
- Visual: `SKIP_TMS_DIAGRAMAS_POR_FLUXO_V4_2_1.pdf` e `SKIP_TMS_DIAGRAMAS_PNG_V4_2_1.zip`

