# SKIP TMS — Vectra Cargo (v4.2)

## Novidades v4.2
- **ANTT:** somente **Tabela A (Lotação)** (cargo_type default “Carga Geral”)
- **ICMS:** UF origem×destino (`key = UF-UF`) — **informativo**
  - `icms_valor_estimado = percent × freight_value`
  - não soma ao frete

## Supabase (SQL)
### Base
1) `SUPABASE_SCHEMA_V4.sql`
2) `SUPABASE_SEED_V4.sql`

### Referências do motor (v4.2)
1) `SUPABASE_MIGRATION_V4_2.sql`
2) `SUPABASE_REFERENCE_DATA_LOADER_V4_2.sql`
3) `SUPABASE_REFERENCE_DATA_V4_2.sql`
