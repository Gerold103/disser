
                     МАГИСТЕРСКАЯ ДИССЕРТАЦИЯ                        


                              6 курс                              

                                                         Выполнил:
                                     Шпилевой Владислав Дмитриевич

                                             Научный руководитель:
                                                     Волканов Д.Ю.

                           Москва, 2018

------------------------------------------------------------------
Исходный код: https://github.com/tarantool/tarantool/commit/7fbfab5bb3ca17cda30233291d4bd5be665265e2
или взять из архива source.zip.

Сборка и запуск согласно инструкциям в readme и с сайта
tarantool.io, а именно:

----------------------------- Сборка -----------------------------

# скачать исходники
$ git clone https://github.com/tarantool/tarantool.git
# или распаковать архив source.zip.

$ cd tarantool
$ git submodule update --init --recursive
$ cmake .
$ make -j

-------------------------- Запуск тестов -------------------------

$ cd test
$ python test-run --force

------------------ Запуск интерактивной консоли ------------------

$ pwd #например, из корня исходников
-- .../tarantool
$ ./src/tarantool

В консоли запуск тарантула на определенном порту:

tarantool> box.cfg{listen = <ваш порт>}

-------------------- Использование алгоритма ---------------------

Далее, пользуясь API, представленном на сайте tarantool.io, можно
создавать таблицы и индексы на движке Vinyl. Чтобы начал работать
алгоритм, все вторичные индексы должны быть неуникальны.

Пример:

box.cfg{}
s = box.schema.create_space('test', {engine = 'vinyl'})
pk = s:create_index('pk')
sk1 = s:create_index('sk1', {unique = false, parts = {{2, 'unsigned'}}})
sk2 = s:create_index('sk2', {unique = false, parts = {{3, 'unsigned'}}})

s:replace{1, 1, 1}
s:replace{2, 2, 2}
s:delete{1}
...
