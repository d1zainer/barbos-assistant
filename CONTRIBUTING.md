# Правила работы с репозиторием

## Ветки

| Ветка       | Назначение                                                          |
|-------------|---------------------------------------------------------------------|
| `main`      | Только то, что готово к использованию. Ничего не коммитим напрямую.  |
| `dev`       | Интеграционная ветка. Сюда сливаются задачи. Напрямую тоже не пишем. |
| `feature/*` | Новая функциональность.                                              |
| `fix/*`     | Исправление бага.                                                    |
| `chore/*`   | Инфраструктура, конфиги, зависимости, документация.                  |

Имя ветки — в kebab-case, по сути задачи: `feature/whisper-recognizer`,
`fix/vad-silence-timeout`, `chore/repo-hygiene`.

## Цикл работы над задачей

```bash
git checkout dev
git pull
git checkout -b feature/<название>

# ...работа, коммиты...

git push -u origin feature/<название>
```

Слияние в `dev` — merge-коммитом, чтобы в истории была видна граница задачи:

```bash
git checkout dev
git merge --no-ff feature/<название>
git push
git branch -d feature/<название>
git push origin --delete feature/<название>
```

## Релиз в `main`

`dev` сливается в `main`, когда этап целиком работоспособен и проверен:

```bash
git checkout main
git merge --no-ff dev
git tag -a v0.1.0 -m "Speech lab"
git push --follow-tags
```

## Коммиты

Conventional Commits:

```
feat(voice): PvRecorder-реализация IAudioCapture
fix(vad): не сбрасывался счётчик тишины после максимальной длины фразы
chore(build): Directory.Build.props с TreatWarningsAsErrors
docs(readme): описание режима benchmark
```

Тип: `feat` | `fix` | `chore` | `docs` | `refactor` | `test` | `perf`.
Область (`voice`, `contracts`, `speechlab`, `build`) — по проекту, которого
касается изменение.

## Что не попадает в репозиторий

- модели Whisper (`*.bin`) — качаются отдельно, путь задаётся в конфиге;
- записанные образцы речи (`samples/`, `*.wav`) и отчёты стенда (`results/`);
- локальные настройки IDE (`.idea/`, `.vs/`) и `appsettings.Local.json`.