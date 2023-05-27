### Домашнее задание «Функции — использование встроенных и создание собственных»


Я работаю секретарем и мне постоянно приходят различные документы. Я должен быть очень внимателен чтобы не потерять ни один документ. Каталог документов хранится в следующем виде:
```
      documents = [
        {"type": "passport", "number": "2207 876234", "name": "Василий Гупкин"},
        {"type": "invoice", "number": "11-2", "name": "Геннадий Покемонов"},
        {"type": "insurance", "number": "10006", "name": "Аристарх Павлов"}
      ]
```
Перечень полок, на которых находятся документы хранится в следующем виде:
```
      directories = {
        '1': ['2207 876234', '11-2'],
        '2': ['10006'],
        '3': []
      }
```


### Задача №1

Необходимо реализовать пользовательские команды, которые будут выполнять следующие функции:

- p – people – команда, которая спросит номер документа и выведет имя человека, которому он принадлежит;
- s – shelf – команда, которая спросит номер документа и выведет номер полки, на которой он находится;
- l– list – команда, которая выведет список всех документов в формате passport "2207 876234" "Василий Гупкин";
- a – add – команда, которая добавит новый документ в каталог и в перечень полок, спросив его номер, тип, имя владельца и номер полки, на котором он будет храниться. Корректно обработайте ситуацию, когда пользователь будет пытаться добавить документ на несуществующую полку.

Внимание: p, s, l, a - это пользовательские команды, а не названия функций. Функции должны иметь выразительное название, передающие её действие.

### Решение
```Python
def work_dokument(command):
  if command == "p":
    number = input('Введите номер документа: ')
    flag = 0
    for document in documents:
      if(document['number']) == number:
        print(document['name'])
        flag = 1
    if flag == 0:
      print("Документ не найден")
    
  if command == "s":
    number = input('Введите номер документа: ')
    flag = 0
    for key, value in directories.items():
        if number in value:
          print("Документ находится на полке номер - ", key)
    if flag == 0:
      print("Документ не найден")
      
  if command == "l":
    for document in documents:
      print(document['type'], document['number'], document['name'])
      
  if command == "a":
    documents.append({"type": input("Введите тип: "), "number": input("Введите номер: "), "name": input("Введите имя: ")})
    while True:
      directory_number = input("Введите номер полки: ")
      if directory_number in directories:
        directories.get(directory_number).append(documents[-1]['number'])
        break
      else:
        print("Такой полки не существует")

print("p – people, s – shelf, l– list, a – add")

command = input("Введите команду: ")

work_dokument(command)
```


### Задача №2

- d – delete – команда, которая спросит номер документа и удалит его из каталога и из перечня полок. Предусмотрите сценарий, когда пользователь вводит несуществующий документ;
- m – move – команда, которая спросит номер документа и целевую полку и переместит его с текущей полки на целевую. Корректно обработайте кейсы, когда пользователь пытается переместить несуществующий документ или переместить документ на несуществующую полку;
- as – add shelf – команда, которая спросит номер новой полки и добавит ее в перечень. Предусмотрите случай, когда пользователь добавляет полку, которая уже существует.;

### Решение
```Python
def work_dokument_2(command):
  if command == "d":
    number = input('Введите номер документа: ')
    flag = 0
    for document in documents:
      if(document['number']) == number:
        documents.remove(document)
        for value in directories.keys():
          for item in directories.get(value):
            if number == item:
              directories.get(value).remove(number)
        flag = 1
    if flag == 0:
      print("Документ не найден")
  
  if command == "m":
    while True:
      document_number = input("Введите номер документа: ")
      flag = 0
      for document in documents:
        if(document['number']) == document_number:
          flag = 1
      if flag == 1:
        break
      else:
        print("Такого документа не существует")
    while True:
      directory_number = input("Введите номер полки: ")
      if directory_number in directories:
        break
      else:
        print("Такой полки не существует!")

    for value in directories.keys():
      for item in directories.get(value):
        if document_number == item:
          directories.get(value).remove(document_number)
          
    directories.get(directory_number).append(document_number)
    
  if command == "as":
    while True:
      directories_number = input("Введите номер полки: ")
      if directories_number in directories:
        print("Такая полка уже существует!")
      else:
        directories[directories_number] = ''
        break

print("d – delete, m – move, as – add shelf")

command = input("Введите командру: ")

work_dokument_2(command)yh n
```

!!!

```Python
def get_name_by_number():
    number = input('Введите номер документа: ')
    for doc in documents:
        if doc['number'] == number:
            print('{0}'.format(doc['name']))
            break
    else:
        print('Отсутствуют документы с таким номером')


def show_documents():
    for doc in documents:
        print(doc['type'], doc['number'], doc['name'])
    for key, values in directories.items():
        print(key, '->', values)


def get_directory_by_number():
    number = input('Введите номер документа: ')
    for directory, list_docs in directories.items():
        if number in list_docs:
            print('Документ с номером {0} находится на полке {1}'.format(number, directory))
            break
    else:
        print('Отсутствуют документы с таким номером')


def add_document():
    number = input('Введите номер документа:')
    name = input('Введите имя и фамилию:')
    doc_type = input('Введите тип документа:')
    directory_number = input('Введите номер полки:')
    if number and name and doc_type and directory_number:
        documents.append({"type": doc_type, "number": number, "name": name})
        if directory_number in directories:
            directories[directory_number].append(number)
        else:
            directories[directory_number] = [number]
    else:
        print('ВНИМАНИЕ! Введены не все данные')


def remove_document():
    person_number = input('Введите номер документа: ')
    bookshelf = ''
    for elem in documents:
        if elem['number'] == person_number:
            documents.remove(elem)
    for number_shelf, number_documents in directories.items():
        if person_number in number_documents:
            number_documents.remove(person_number)
            bookshelf = number_shelf
            break
    print(f'Удален документ с номером: {person_number} из базы данных и убран с полки №{bookshelf}')


def main():
    while True:
        command = input('Введите команду: ')
        if command == 'p':
            get_name_by_number()
        elif command == 'l':
            show_documents()
        elif command == 's':
            get_directory_by_number()
        elif command == 'a':
            add_document()
        elif command == 'd':
            remove_document()
            print(documents)
            print(directories)
        elif command == 'e':
            break

main()
```
