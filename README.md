## Ответ на вопрос «По какой причине не завершается работа функции main?»

Если я всё верно понял происходит взаимная блокировка потоками друг друга,так как первый поток использует аргумент resourceA и далее ждет resourceB,
а в это время второй поток использует resourceB и ждет освобождения resourceA который в свою очередь блокирован первым потоком.И получается оба потока ждут освобождения 
аргументов которые в это время блокированы другим потоком.Патовая ситуация.
