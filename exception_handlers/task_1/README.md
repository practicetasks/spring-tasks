# Добавляем обработку ошибок в Catsgram

Проект `Catsgram` растёт, и к нему появилось ещё одно требование — сообщения об ошибках должны отображаться в едином
формате:

Склонируйте [репозиторий](https://github.com/practicetasks/catsgram/tree/exception-handlers)

Создайте исключение `IncorrectParameterException`, унаследовав его от `RuntimeException`. В исключение добавьте поле
`parameter`.

Исправьте условия в `PostController` и `PostFeedController`, чтобы для ошибки в конкретном поле вместо
`IllegalArgumentException` пробрасывалось `IncorrectParameterException` с названием ошибочного поля. Например, если
значение
page меньше нуля, должно быть проброшено исключение, в котором поле `parameter` содержит значение `“page”`.

В пакет `kz.runtime.catsgram.controller` добавьте класс `ErrorHandler` — обработчик ошибок. При ошибках должен
возвращаться объект `ErrorResponse`. Добавьте логику по обработке исключений:

- Для исключений `PostNotFoundException` и `UserNotFoundException` должен возвращаться код ответа **404** (_Not found_),
  в поле
  `error` — сообщение из исключения.
- Для исключения `UserAlreadyExistException` — код ответа **409** (_Conflict_), в поле `error` — сообщение из
  исключения.
- Для исключения `InvalidEmailException` — код ответа **400** (_Bad request_), в поле `error` — сообщение из исключения.
- Для исключения `IncorrectParameterException` — код ответа **400** (_Bad request_), в поле `error` — сообщение
  вида `Ошибка с
  полем “наименование поля”`.
- Для любого другого исключения (`Throwable`) — код ответа **500** (_Internal server error_) и сообщение `"Произошла
  непредвиденная ошибка."`.

#### Подсказки

- Для того чтобы возвращать указанный формат ошибки, можно завести отдельный класс `ErrorResponse` с полем `error`. А в
  методах по обработке ошибки указать `ErrorResponse` в качестве возвращаемого типа.
- В классе `ErrorHandler` не забудьте поставить аннотацию `@RestControllerAdvice`.
- Над методом по обработке ошибок необходимо поставить аннотацию `@ExceptionHandler`.
- Вернуть код ответа можно с помощью аннотации `@ResponseStatus`(`код ответа`).
- Выбрасывание исключений с соответствующим полем можно сделать, например, следующим образом:
  ```java
  if (!SORTS.contains(sort)) {
      throw new IncorrectParameterException("sort");
  }
  if (page < 0) {
      throw new IncorrectParameterException("page");
  }
  if (size <= 0) {
      throw new IncorrectParameterException("size");
  }
  ```