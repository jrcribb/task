---
slug: /styleguide/
sidebar_position: 8
---

# スタイルガイド

これは`Taskfile.yml`の公式Taskスタイルガイドです。 このガイドには、Taskfileを整理し、他ユーザーにとっても理解しやすいように保つための基本的な手順が含まれています。

これには一般的なガイドラインが含まれていますが、必ずしも厳密に従う必要はありません。 必要であったり、あるいは違う方法を取りたい場合は自由にしてください。 また、このガイドの改善点について、IssueまたはPull Requestを開くことも自由です。

## `taskfile.yml`ではなく`Taskfile.yml`を使用してください

```yaml
# bad
taskfile.yml


# good
Taskfile.yml
```

これはLinuxユーザーにとって特に重要です。 WindowsとmacOSは大文字と小文字を区別しないファイルシステム持っているので、公式にはサポートされていないにもかかわらず、`taskfile.yml`は正常に動作します。 Linuxでは`Taskfile.yml`だけが動作します。

## Use the correct order of keywords

- `version:`
- `includes:`
- Configuration ones, like `output:`, `silent:`, `method:` and `run:`
- `vars:`
- `env:`, `dotenv:`
- `tasks:`

## インデントにはスペース2つを使用してください

これはYAMLファイルの最も一般的な慣習であり、Taskはそれに倣うものです。

```yaml
# bad
tasks:
    foo:
        cmds:
            - echo 'foo'


# good
tasks:
  foo:
    cmds:
      - echo 'foo'
```

## Separate with spaces the mains sections

```yaml
# bad
version: '3'
includes:
  docker: ./docker/Taskfile.yml
output: prefixed
vars:
  FOO: bar
env:
  BAR: baz
tasks:
  # ...


# good
version: '3'

includes:
  docker: ./docker/Taskfile.yml

output: prefixed

vars:
  FOO: bar

env:
  BAR: baz

tasks:
  # ...
```

## Add spaces between tasks

```yaml
# bad
version: '3'

tasks:
  foo:
    cmds:
      - echo 'foo'
  bar:
    cmds:
      - echo 'bar'
  baz:
    cmds:
      - echo 'baz'


# good
version: '3'

tasks:
  foo:
    cmds:
      - echo 'foo'

  bar:
    cmds:
      - echo 'bar'

  baz:
    cmds:
      - echo 'baz'
```

## Use upper-case variable names

```yaml
# bad
version: '3'

vars:
  binary_name: myapp

tasks:
  build:
    cmds:
      - go build -o {{.binary_name}} .


# good
version: '3'

vars:
  BINARY_NAME: myapp

tasks:
  build:
    cmds:
      - go build -o {{.BINARY_NAME}} .
```

## Don't wrap vars in spaces when templating

```yaml
# bad
version: '3'

tasks:
  greet:
    cmds:
      - echo '{{ .MESSAGE }}'


# good
version: '3'

tasks:
  greet:
    cmds:
      - echo '{{.MESSAGE}}'
```

This convention is also used by most people for any Go templating.

## Separate task name words with a dash

```yaml
# bad
version: '3'

tasks:
  do_something_fancy:
    cmds:
      - echo 'Do something'


# good
version: '3'

tasks:
  do-something-fancy:
    cmds:
      - echo 'Do something'
```

## Use colon for task namespacing

```yaml
# good
version: '3'

tasks:
  docker:build:
    cmds:
      - docker ...

  docker:run:
    cmds:
      - docker-compose ...
```

This is also done automatically when using included Taskfiles.

## Prefer external scripts over complex multi-line commands

```yaml
# bad
version: '3'

tasks:
  build:
    cmds:
      - |
        for i in $(seq 1 10); do
          echo $i
          echo "some other complex logic"
        done'

# good
version: '3'

tasks:
  build:
    cmds:
      - ./scripts/my_complex_script.sh
```