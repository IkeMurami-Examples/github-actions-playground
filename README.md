# github-actions-playground
Playground to check working with Github actions

## Статьи вспомогательные


- https://stackoverflow.com/questions/75251487/passing-secrets-as-output-between-jobs-in-a-github-workflow
- https://github.com/orgs/community/discussions/13082
- https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions
- Проверить двойное кодирование https://github.com/orgs/community/discussions/25225#discussioncomment-6776295



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

Этот подход работает при передачи секретов между step'ами, но не позволяет передать секрет в другую job'у: секрет выпиливается из output'а. 

У классического применения Github Secrets есть возможность прочитать один и тот же секрет через `secrets.SOME_SECRET`.

## Двойное кодирование (double_encode.yml)

https://github.com/orgs/community/discussions/25225#discussioncomment-6776295



## Вторая попытка: encryption.yml

Шифруем на общем секрете для передачи между job'ами.

Но как передать значение между workflow => шифрование не нужно, все таки нужно свое хранилище секретов.

Если значение должно попадать в env'ы, то расшифровывать следует внутри step'а, так как при подстановки в env, значение будет видно

## Третья попытка: custom action

Тут можно использовать как сторонние хранилища, например: hashicorp vault, а можно написать свой custom action, который будет класть секрет в Github Secrets.

+ Рассуждения, почему не получится через отдельный workflow (re-usable wf — это job'а, а значит снова попадаем в проблему как передать секрет между job'ами)

