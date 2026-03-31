```ampcss
Из кого состояла команда разработки и роль в команде

Какая сложность получения элемента из списка, кортежа. - (O(n), O(1))

Как использовать другой шаблон в django шаблоне - тэг include

Варианты оптимизации sql запросов в django - select_related, prefetch_related, only, ...

Для чего нужна индексация БД, плюсы и минусы - Для ускорения поиска, быстрее поиск, долше вставка.

какие паттерны использовал? -

Был опыт работы в фронтенд, отрисовки компонентов?

опыт nginx

какие типы данных изменяемые и нет в python

опыт работы с git, перечислить основные команды. Как решить конфликт?

опыт работы на серевере *nix подобных

Опыт с docker-compose? как обратиться из одного контейнера docker в другой

опыт работы с очередями celery rabbitmq -

С какими линтерами работал -
```

```ampcss
Ниже написан код, нужно провести код ревью

import pandas as pd
df = pd.read_csv("data.csv", sep=",", header=None)
if df.values.sum() == 10:
  print(True)
else:
  print(False)
```

```ampcss
Дана скобочная последовательность, определить правильная она или нет?.

def is_correct_sequence(data: str) -> bool:
    ...

# correct = "()"
# correct = "(()())"
# correct = "(()(()))"
# incorrect = ")()"
# incorrect = "(())("
# incorrect = "))(("
```

```ampcss
Есть модель Comment, как сделать уникальными каждое сочетание title и text, зная что text это огромное текстовое поле которое может быть любой длины

class Comment(models.Model):
  class Meta:
    verbose_name = "Comment"
    verbose_name_plural = "Comments"
    ordering = ("-pk",)

title = models.CharField("Title", max_length=4000, null=True)
text = models.TextField("Text")
```