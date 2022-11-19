Напишите программу, которая выберет из базы фразы по условию.
Вашему решению будет доступен файл базы данных с таблицей stories такой структуры (показана БД для примеров):
id, name, 	priority, number_of_listeners, place
PIC

Формат ввода
Вводится имя файла, потом союз, которым соединяются условия, затем несколько условий, каждое с новой строки.
Формат вывода
Вывести фразы, удовлетворяющие всем условиям, в порядке убывания приоритета (поле priority).
Пример 1
Ввод
silence.db
OR
place == 'tavern'
name in ('Livesey', 'Trelawny')
Вывод
Hey, down there on the lower deck
Are you addressing me, sir?

Пример 2
Ввод
tales.db
AND
priority >= 5
number_of_listeners BETWEEN 2 AND 17
	
Вывод
Everyone was instantly silent
Are you addressing me, sir?

Примечания
Файлы из примеров можно загрузить здесь: https://yastatic.net/s3/lyceum/task_files/2020_E2L2_QiEDV.zip








import sqlite3
import sys
name = input()
soius = input()
lst = list(map(str.strip, sys.stdin))
stroka = ""
counter = 0
for x in lst:
    stroka += f"{x} "
    if counter != len(lst) - 1:
        stroka += f"{soius} "   # Заполнение строки запроса
    counter += 1

with sqlite3.connect(name) as con:
    cur = con.cursor()
    answer = cur.execute(f"""SELECT phrase, priority FROM stories WHERE {stroka}""").fetchall()
    answer = sorted(answer, key=lambda i: i[-1], reverse=True)

for j in answer:
    print(j[0])
