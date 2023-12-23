# github-actions-playground
Playground to check working with Github actions

## init.yml

У нас есть job'а (а точнее step в ней), которая генерирует секрет. И нам этот секрет надо передать в следующий step или следующую job'у.
Нам ее может потребоваться передать как через ENV'ы, так и через output'ы отдельного шага или output'ы всей job'ы.

## problem.yml

В чем проблема:

- При подстановке в `${{  }}` значение палится в логах
- Палятся env'ы

## secrets.yml

Как себя ведут обычные секреты в Github Actions.

## Первая попытка решить: add_mask.yml

Добавляем маскирование секретов в логах: https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#example-masking-a-generated-output-within-a-single-job

