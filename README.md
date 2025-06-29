КАЛЬКУЛЯТОР ВЫРАЖЕНИЙ m() / M()
(Индивидуальная работа № 2 по дисциплине «Язык программирования Python»)
вариант 1

Семешко Андрей ИТ-5

──────────────────────────────────────────────────────────────────────────────
1. Постановка задачи
──────────────────────────────────────────────────────────────────────────────
Требуется вычислить значение корректного выражения, содержащего две
бинарные операции:

m(a,b) – минимум двух положительных целых
M(a,b) – максимум двух положительных целых

Операции допускают произвольную вложенность, пробелы в строке
отсутствуют, аргументы – только положительные целые.
Пример: M(15,m(16,8)) → 15 

──────────────────────────────────────────────────────────────────────────────
2. Выбор структуры данных
──────────────────────────────────────────────────────────────────────────────
Для обработки выражения используются два стека:
• ops – хранятся встреченные операции m и M
• vals – хранятся числа и промежуточные результаты

Почему именно стек
• Порядок LIFO естественно отражает закрытие скобок: когда встречаем
символ ), два верхних значения из vals и верхняя операция из ops
относятся к данной паре скобок.
• Операции push/pop выполняются за O(1) времени и памяти, что
гарантирует линейную сложность всего алгоритма.
Стеки реализованы собственным классом Stack на односвязном списке узлов
Node(value, next).

──────────────────────────────────────────────────────────────────────────────
3. Алгоритм решения
──────────────────────────────────────────────────────────────────────────────

Предварительная проверка строки
‒ пустая ли строка;
‒ встречаются ли недопустимые символы;
‒ сбалансированы ли скобки.

Однопроходный разбор слева направо:
‒ если встретили цифру – считываем всё число и кладём
в vals;
‒ если встретили символ m или M – помещаем его
в ops;
‒ если встретили символ ) – достаём из vals два аргумента,
из ops операцию, вычисляем min или max и кладём результат
обратно в vals;
‒ символы ( и , пропускаем.

После прохода в vals должно остаться ровно одно значение, а ops
должен быть пуст; иначе фиксируется ошибка «Некорректное выражение».
Сложность алгоритма – O(n) по времени и O(d) по памяти, где n –
длина строки, d – максимальная глубина вложенности. 

──────────────────────────────────────────────────────────────────────────────
4. Обработка исключений
──────────────────────────────────────────────────────────────────────────────
• Функция validate(expr) выявляет пустую строку, недопустимые символы
и несбалансированные скобки ещё до запуска вычислений.
• Метод Stack.pop() проверяет извлечение из пустого стека и
выбрасывает ValueError("Стек пуст").
• После завершения разбора выражения проверяем, что в стеке остался
один элемент; иначе – ValueError("Некорректное выражение").
• Интерактивная оболочка перехватывает все ValueError и выводит
понятное сообщение пользователю вместо трассировки стека.
Так каждая возможная ошибка фиксируется как можно ближе к месту её
возникновения и не приводит к аварийному завершению программы.

──────────────────────────────────────────────────────────────────────────────
5. Классы и ООП-подход
──────────────────────────────────────────────────────────────────────────────
Node – атомарный узел списка, хранит значение и ссылку на следующий
элемент.

Stack – контейнер LIFO с методами push, pop, len.
Инкапсуляция: внутренние поля head и size не используются напрямую.
Полиморфизм: определён метод len, поэтому объект Stack корректно
работает со встроенной функцией len.
Наследование не требуется, но класс легко расширить, например создав
LoggingStack с переопределением push/pop.

──────────────────────────────────────────────────────────────────────────────
6. Тестирование
──────────────────────────────────────────────────────────────────────────────
В отчёте приведено десять положительных и отрицательных примеров,
включая стресс-выражение длиной около восьмидесяти символов
с шестью уровнями вложенности, а также три сценария с ошибками
(лишняя ), недопустимый символ, несбалансированные скобки). Все они
демонстрируют корректную работу обработчиков исключений. 

──────────────────────────────────────────────────────────────────────────────
7. Запуск проекта
──────────────────────────────────────────────────────────────────────────────
git clone https://github.com/EnotikonZ/IKM.git
cd IKM/v1
python minmax_calc.py – интерактивный режим
echo "M(15,m(16,8))" | python minmax_calc.py – пример запуска из конвейера
Пустая строка завершает работу калькулятора.

──────────────────────────────────────────────────────────────────────────────
Автор: Семешко Андрей ИТ-5
