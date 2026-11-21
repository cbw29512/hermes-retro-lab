# Hermes Retro Lab — State contract v1

The state file `state.json` is the durable progress record. Read and validate it **before** changing any project files. Keep this shape until an explicit, tested migration is made.

| Key | Type | Description |
|---|---|---|
| `schema_version` | string | Exactly `1.0` |
| `experiment` | object | `name`, `status`, `started_at`, `completed_at` |
| `environment` | object | `type`, `provisioned`, `host_access`, `inference_endpoint_ready`, `repository_scope` |
| `current_product` | object | `slug`, `title`, `category`, `stage`, `description`, `source_paths`, `preview_paths`, `package_path` |
| `last_run` | object | `run_id`, `started_at`, `ended_at`, `result`, `summary`, `tests` |
| `metrics` | object | `product_candidates`, `products_packaged`, `tests_passed`, `tests_failed`, `verified_sales_count`, `verified_net_revenue_usd` |
| `blockers` | array[string] | Unresolved and evidenced blockers |
| `next_action` | string | One next concrete task |

State values:
- `experiment.status`: `ready`, `running`, `blocked`, `completed`.
- `current_product.stage`: `not_started`, `research`, `building`, `testing`, `packaged`, `blocked`.
- `last_run.result`: `not_run`, `completed`, `blocked`, `error`.
- Each `last_run.tests` entry: `name` (string), `status` (`passed`, `failed`, `not_run`), `evidence` (string).
- Timestamps must be ISO 8601 with offset or `null`; counters/currency nonnegative.

Integrity:
1. Never record a delivered product unless the package and preview exist and pass validation.
2. A proposed price or sale listing does not count as revenue.
3. Keep verified sales/revenue at zero without external transaction evidence.
4. Save long reports in `reports/`. Preserve unknown keys and record failure details; no fabricated success.
5. Publish only non-sensitive state suitable for a public repository.
