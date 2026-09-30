# UPSTREAM.md — зеркало над brake71/1c-ssl-skills

Это зеркало живого апстрима (MIT, БСП-скилл `bsp`). Его единственное
назначение — держать **минимальный diff** над апстрим-контентом, чтобы
обновления забирались явным решением, а не утекали сами.

## Наш слой (всё, что отличается от апстрима)

| Файл | Что |
|---|---|
| `skills/bsp/SKILL.md` | frontmatter: добавлены `when_to_use` и `license` (нативные ключи ZCode); тело НЕ меняется |
| `deploy.json` | контракт v1 sot-zcode-marketplace (вендоринг `skills/bsp → .zcode/skills/bsp`) |
| `UPSTREAM.md` | этот файл |

`agents/openai.yaml` в поставку не входит (Codex-специфичный, в mapping
не попадает). Дев/релизная инфраструктура апстрима (`ci/`, `evals/`,
`tests/`, `reports/`, `plans/`, `vendor/`, `install.*`) в проекты
не вендорится.

## Синк с апстримом

```bash
git fetch upstream --tags
git merge upstream/main        # конфликтная поверхность = frontmatter SKILL.md
# линтер под слоты ZCode:
python3 <1c-zcode>/scripts/lint_skill_frontmatter.py skills/
# затем: тег vX.Y.Z → re-pin в sot-zcode-marketplace/marketplace.json →
#        deploy-plugin.py update в проектах
```

Правило гигиены: всё, что выражается обвязкой потребителя (роутер-скилл
контура, workflow) — в обвязке, не в теле форка.

## Журнал синков

| Дата | Коммит апстрима | Версия апстрима | Что менялось у нас |
|---|---|---|---|
| 2026-10-01 | `3785579` (v0.12) | v0.12 | создание зеркала: frontmatter + deploy.json + UPSTREAM.md |
