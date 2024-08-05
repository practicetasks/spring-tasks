# Задача

- Прочитайте код. Сейчас тест метода `testSaveOrder` падает, потому что в один из методов передано неверное значение
  аргумента. Попробуйте исправить его так, чтобы он заработал (Что тест пройден можно увидеть в выводе результатов.
  Вывод должен включать две строки: `1 tests started` и `1 tests successful`.).
- Добавьте в тест проверку того, что мы не забыли уменьшить доступное количество книг на складе после создания заказа.
  Обратите внимание, что за это отвечает метод `bookService.decreaseBookAvailableAmount`.

```java
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

import java.time.LocalDate;

import static org.mockito.ArgumentMatchers.*;

public class OrderServiceTest {
    @Test
    void testSaveOrder() {
        OrderService orderService = new OrderService();

        CustomerService customerService = Mockito.mock(CustomerService.class);
        BookService bookService = Mockito.mock(BookService.class);
        OrderDao orderDao = Mockito.mock(OrderDao.class);

        orderService.setCustomerService(customerService);
        orderService.setBookService(bookService);
        orderService.setOrderDao(orderDao);

        Mockito.when(customerService.customerIsBlocked(anyInt())).thenReturn(false);
        Mockito.when(customerService.getCustomerAddress(anyInt())).thenReturn("адрес");

        LocalDate orderDate = LocalDate.of(2022, 4, 12);
        LocalDate deliveryDate = LocalDate.of(2022, 4, 14);
        orderService.saveOrder(2, 5, 1, orderDate);

        Mockito.verify(orderDao, Mockito.times(1))
                .saveOrder(2, "адрес", 5, 1, deliveryDate);
        Mockito.verify(customerService, Mockito.times(1))
                .getCustomerAddress(5);
    }
}
```

```java
import java.time.LocalDate;

public class OrderService {
    CustomerService customerService;
    BookService bookService;
    OrderDao orderDao;

    public void saveOrder(int customerId, int bookId, int amount, LocalDate orderDate) {
        if (customerService.customerIsBlocked(customerId)) {
            throw new CustomerIsBlockedException();
        }

        String customerAddress = customerService.getCustomerAddress(customerId);
        LocalDate deliveryDate = orderDate.plusDays(2);

        orderDao.saveOrder(customerId, customerAddress, bookId, amount, deliveryDate);
        bookService.decreaseBookAvailableAmount(bookId, amount);
    }

    public void setCustomerService(CustomerService customerService) {
        this.customerService = customerService;
    }

    public void setOrderDao(OrderDao orderDao) {
        this.orderDao = orderDao;
    }

    public void setBookService(BookService bookService) {
        this.bookService = bookService;
    }
}
```

```java
public class BookService {
    public void decreaseBookAvailableAmount(int bookId, int amount) {
        //уменьшаем доступное количество единиц в БД на 'amount'
    }
}
```

```java
public class CustomerService {
    public boolean customerIsBlocked(int customerId) {
        //проверяем, нет ли у пользователя неоплаченных заказов
        return false;
    }

    public String getCustomerAddress(int customerId) {
        //получаем адрес доставки из базы данных
        return "address";
    }
}
```

```java
import java.time.LocalDate;

public class OrderDao {
    public void saveOrder(int customerId, String deliveryAddress, int bookId, int amount, LocalDate deliveryDate) {
        //сохраняем заказ в базу данных
    }
}
```

```java
public class CustomerIsBlockedException extends RuntimeException {
}
```

_подсказки ниже_

<br><br><br><br><br><br><br><br><br><br><br><br><br>

- Обратите внимание на проверку метода `getCustomerAddress` мок-объекта `customerService`.
- Для верификации вызова нужно использовать статический метод `Mockito.verify`. Нужно проверить, что метод
  `decreaseBookAvailableAmount` мок-объекта bookService был вызван один раз и было передано правильное количество книг.
