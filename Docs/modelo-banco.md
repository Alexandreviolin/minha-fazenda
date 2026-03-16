# Modelagem do Banco de Dados - Minha Fazenda

O banco será o **Cloud Firestore** (NoSQL). A estrutura favorecerá leituras rápidas (`O(1)`), dado que ferramentas de dashboard Web lêem muito e Mobile frequentemente baixa toda coleção de base para cache.

## Estrutura de Collections

### `users`
Dados dos peões, técnicos e gerentes.
* `id` (string, Auth UID)
* `name` (string)
* `role` (string - "admin", "operator", "vet")
* `farm_id` (string, referência à fazenda)
* `created_at` (timestamp)

### `farms`
Perfil Multi-tenant se o app escalar (ou apenas uma única default).
* `id` (string)
* `name` (string)
* `owner_id` (string)
* `settings` (map)

### `animals`
Rastreio individual.
* `id` (string)
* `farm_id` (string)
* `tag` (string, brinco ou chip único)
* `type` (string - "bovino_corte", "bovino_leite", "suino", etc)
* `breed` (string)
* `birth_date` (timestamp)
* `gender` (string - "M", "F")
* `status` (string - "active", "sold", "dead")
* `current_weight` (number)
* `batch_id` (string, lote atual onde está)
* `parents` (map - {sire_id, dam_id} para genealogia)

### `batches` (Lotes)
Agrupamento prático (ex: "Bezerros Nelore Inverno 26").
* `id` (string)
* `farm_id` (string)
* `name` (string)
* `animal_type` (string)
* `location` (string)
* `notes` (string)
* `head_count` (number, redundante para queries rápidas do dashboard)

### `events` (Eventos/Histórico)
Subcollection ou coleção raiz com logs diários de tudo que acontece com o animal/lote. Isso alimenta os gráficos.
* `id` (string)
* `farm_id` (string)
* `entity_type` (string - "animal" ou "batch")
* `entity_id` (string)
* `type` (string - "weight", "vaccine", "medicine", "feed", "move", "death")
* `value` (number ou string - ex: peso, qtd de remédio)
* `product_id` (string, opcional, ID do remédio/ração)
* `date` (timestamp)
* `operator_id` (string, quem efetuou a anotação)

### `inventory` (Estoque/Insumos)
Vacinas, Rações e Remédios.
* `id` (string)
* `farm_id` (string)
* `name` (string)
* `category` (string - "vaccine", "feed", "medicine")
* `unit` (string - "kg", "ml", "dose")
* `current_quantity` (number)
* `min_quantity_alert` (number)
* `cost_per_unit` (number, para projeções de lucro do Dashboard)
