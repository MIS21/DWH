# Инструкция по запуску проекта

1. **Скопируйте репозиторий**:
   - Склонируйте репозиторий на ваш компьютер.

2. **Запустите контейнеры**:
   - Выполните в терминале команду:
     ```bash
     docker-compose up -d
     ```

3. **Подключитесь к базе данных**:
   - Используйте DBeaver для подключения к контейнеру с базой данных.
   - Создайте таблицы и заполните их данными, выполнив соответствующие SQL-скрипты.
   - Модифицируйте таблицу `dwh.f_orders`, замените её определение на это:

```sql
DROP TABLE IF EXISTS dwh.f_orders;
CREATE TABLE dwh.f_orders (
    order_id BIGINT GENERATED ALWAYS AS IDENTITY NOT NULL,
    product_id int8 NOT NULL,
    craftsman_id int8 NOT NULL,
    customer_id int8 NOT NULL,
    order_created_date DATE NULL,
    order_completion_date DATE NULL,
    order_status VARCHAR NOT NULL,
    load_dttm timestamp NOT NULL,
    CONSTRAINT orders_pk PRIMARY KEY (order_id),
    CONSTRAINT orders_craftsmans_fk FOREIGN KEY (craftsman_id) REFERENCES dwh.d_craftsmans(craftsman_id) ON DELETE restrict,
    CONSTRAINT orders_customers_fk FOREIGN KEY (customer_id) REFERENCES dwh.d_customers(customer_id) ON DELETE restrict,
    CONSTRAINT orders_products_fk FOREIGN KEY (product_id) REFERENCES dwh.d_products(product_id) ON DELETE restrict
);

COMMENT ON TABLE dwh.f_orders IS 'Фактовая таблица с заказами';
COMMENT ON COLUMN dwh.f_orders.order_id IS 'идентификатор заказа';
COMMENT ON COLUMN dwh.f_orders.product_id IS 'идентификтор товара ручной работы';
COMMENT ON COLUMN dwh.f_orders.craftsman_id IS 'идентификатор мастера';
COMMENT ON COLUMN dwh.f_orders.customer_id IS 'идентификатор заказчика';
COMMENT ON COLUMN dwh.f_orders.order_created_date IS 'дата создания заказа';
COMMENT ON COLUMN dwh.f_orders.order_completion_date IS 'дата выполнения заказа';
COMMENT ON COLUMN dwh.f_orders.order_status IS 'статус выполнения заказа (created, in progress, delivery, done)';
COMMENT ON COLUMN dwh.f_orders.load_dttm IS 'дата и время загрузки';
```
4. **Работа с Spark Notebook**:
   - В контейнер `spark_notebook` вмонтирована папка `spark_scripts`, содержащая блокнот (notebook).
   - Откройте блокнот в Jupyter Notebook


5. **Выполнение скриптов**:
   - **Первая ячейка** содержит код для заполнения таблиц измерений и фактов. Выполните её.
   - **Вторая ячейка** содержит код для заполнения витрины данных. Выполните её.

6. **Проверка результата**:
   - Результат выполнения можно проверить в DBeaver, подключившись к базе данных и просмотрев содержимое таблиц.
