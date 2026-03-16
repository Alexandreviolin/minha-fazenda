# Documentação de APIs Internas / Cloud Functions

A maioria do tráfego será feito diretamente via **Firebase SDK Client-Side**, eliminando a necessidade de dezenas de rest APIs. O modelo de segurança do Firestore cuidará da autorização.
Porém, algumas ações complexas de negócio requerem a segurança do Server-Side (Cloud Functions).

## Endpoints/Functions (Firebase Callable Functions)

### `calculateBatchCost(batch_id: string)`
* **Descrição**: Calcula o custo total alocado a um lote específico somando todos os `events` de nutrição e vacinação vezes o `cost_per_unit` de seu estoque respectivo.
* **Entrada**: `{ "batch_id": "ABC1234" }`
* **Saída**: `{ "total_cost": 5400.50, "currency": "BRL", "head_cost": 54.00 }`
* **Auth**: Requer role `admin` ou `vet`.

### `processEventTrigger` (Firestore OnCreate Trigger)
* **Descrição**: Acionado automaticamente sempre que um documento na coleção `events` é gerado pelo peão no app (ex: aplicou vacina).
* **Fluxo**:
  1. Identifica se é do type `vaccine` ou `feed`.
  2. Subtrai o `value` consumido da coleção `inventory`.
  3. Se o novo estoque bater abaixo de `min_quantity_alert`, posta um alerta geral para os Admins (via push ou coleção de notificações).

### `syncOfflineAnimals(batch: Array)`
* **Descrição**: Função para bulk commit quando o app mobile subir centenas de eventos que estavam em cache. (Mesmo podendo ser feito via batch SDK, encapsular na function protege contra flood/race conditions em estoque).

## Regras de Segurança Básicas (Firestore Rules)
* `users`, `farms`: Leitura livre para o mesmo `farm_id`. Escrita só por admin.
* `animals`, `batches`: Leitura livre (mesmo `farm_id`). Operadores podem escrever apenas eventos atrelados a animais, sem deletá-los.
* `inventory`: Apenas admins podem alterar o preço manual de custo. Operadores modificam quantidade indiretamente ao gerar eventos.
