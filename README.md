# Лабораторная работа №2
## Выполнил студент группы БСТ2201 Боджанг М.Л.

Создадим два класса для работы

class Stack:

    # инициализация
    
    def __init__(self):

    
        self.items = []
        
    # проверка на пустоту
    
    def isEmpty(self):
    
        return self.items == []
        
    # добавить элемент
    
    def push(self, item):
    
        self.items.append(item)
        
    # удалить элемент
    
    def pop(self):
    
        return self.items.pop()
        
    # развернуть 
    
    def reverse(self):
    
        self.items.reverse()
        
# класс для работы с Deque

class Deque:

    # инициализация
    
    def __init__(self):
    
        self.items = []
        
    # проверка на пустоту
    
    def isEmpty(self):
    
        return self.items == []
        
    # добавить вправо
    
    def append(self, item):
    
        self.items.append(item)
        
    # добавить влево
    
    def appendleft(self, item):
    
        self.items.insert(0, item)
        
    # удалить справа
    
    def pop(self):
    
        return self.items.pop()
        
     # удалить слева
     
    def popleft(self):
    
        return self.items.pop(0)
        
    # вернуть элемент без удаления
    
    def peek(self):
    
        return self.items[-1]
        
     # развернуть 
     
    def reverse(self):
    
        self.items.reverse()
        
    # получить длину
    
    def __len__(self):
    
        return len(self.items)


        # Функция для чтения текста из файла
        
def read_file(filename):

    with open(filename, 'r', encoding = "utf-8") as file:
    
        return [row.strip() for row in file]
        

# Функция для загрузки в файла

def save_to_file(filename, result):

    with open(filename, 'w', encoding = "utf-8") as f:
    
        if isinstance(result, str):
        
            f.write(result + "\n")
            
        else:
        
            for item in result:
            
                f.write(str(item) + "\n")


                ### Задание №1
                
Отсортировать строки файла, содержащие названия книг, в алфавитном порядке с использованием двух деков.


# функция для сортировки названий книг

def sort_books(input_file_path):

  deq, sort_deq = [Deque() for _ in range(2)]
  
  [deq.append(book.strip()) for book in books]
  
  while not deq.isEmpty():
  
    element = deq.pop()
    
    # пока sort_deq не пустой и element меньше чем последний в sort_deq
    
    while not sort_deq.isEmpty() and element < sort_deq.peek():
    
      # добавляем в deq последний элемент sort_deq
      
      deq.append(sort_deq.pop())
      
    sort_deq.append(element)
    
  # вызов функции сохранения в файл
  
  save_to_file("test1.txt", sort_deq.items)
  
  while not sort_deq.isEmpty():
  
    print(sort_deq.popleft())
    
# вызов функции чтения из файл

books = read_file("task1.txt")

sort_books(books)


### Задание №2

Дек содержит последовательность символов для шифровки сообщений. Дан текстовый файл, содержащий зашифрованное сообщение. Пользуясь деком, расшифровать текст. 
Известно, что при шифровке каждый символ сообщения заменялся следующим за ним в деке по часовой стрелке через один.


# функция для расшифровки текста

def decode_text(input_file_path):

  deq = Deque()
  
  [deq.append(letter) for letter in 'абвгдеёжзийклмнопрстуфхцчшщъыьэюя']
  
  decrypted_text = ""
  
  # функция для расшифровки каждого символа
  
  def decryptText(variable):
  
    for letter in range(len(deq)):
    
      symbol = deq.popleft()
      
      # если наш извлеченный символ из дека == сравниваемому из текста
      
      if symbol == variable:
      
        # добавляем наш извлеченный символ из дека в конец дека
        
        deq.append(symbol)
        
        # извлечем след. символ после symbol
        
        next_symbol = deq.popleft()
        
        # посместим его в конец списка
        
        deq.append(next_symbol)
        
        # вернем последний извлченный символ для добавления в decrypted_text
        
        return next_symbol
        
      deq.append(symbol)

  for string in decode:
  
    for symbol in string.lower():
    
      decode_symbol = decryptText(symbol)
      
      #проверяет, произошло ли шифрование
      
      if decode_symbol:
      
          decrypted_text += decode_symbol
          
      else:
      
          decrypted_text += symbol
          
   вызов функции сохранения в файл
  
  save_to_file("test2.txt", decrypted_text)
  
  return decrypted_text
  
вызов функции чтения из файл

decode = read_file("task2.txt")

decode_text(decode)

### Задание №3

Даны три стержня и n дисков различного размера. Диски можно надевать на стержни, образуя из них башни. Перенести n дисков со стержня А на стержень С, сохранив их первоначальный порядок. При переносе дисков необходимо соблюдать следующие правила:
- на каждом шаге со стержня на стержень переносить только один диск;
- диск нельзя помещать на диск меньшего размера;
- для промежуточного хранения можно использовать стержень В.
Реализовать алгоритм, используя три стека вместо стержней А, В, С. Информация о дисках хранится в исходном файл


def hanoi_tower(input_file_path):


  A, C, B = [Stack() for _ in range(3)]
  
  положим диски в A начиная с n (большего)
  
  [A.push(disk) for disk in range(n, 0, -1)]
  
 динамическое добавление атрибута
 
  A.name,C.name,B.name = "A", "C", "B"
  
  print(f'Состояние стержня A: {A.items}')
  
 функция для движения дисков (решение рекурсией)
 
  def move_disks(n, start, end, middle):
  
    if n == 1:
    
      end.push(start.pop())
      
      print(f'Move disk 1 from rod {start.name} to rod {end.name}')
      
      return
      
    else:
    
      move_disks(n-1, start, middle, end)
      
      print(f'Move disk {n} from rod {start.name} to rod {end.name}')
      
      end.push(start.pop())
      
      move_disks(n-1, middle, end, start)
      
  вызов функции и результата
  
  move_disks(n, A, C, B)
  
  C.reverse()
  
  save_to_file("test3.txt", C.items)
  
  C.reverse()
  
  return print(f'Состояние стержня C: {C.items}')
  
n = int(read_file("task3.txt")[0])

hanoi_tower(n)

### Задание №4

Дан текстовый файл с программой на алгоритмическом языке. За один просмотр файла проверить баланс круглых скобок в тексте, используя стек.

def balance_round_brackets(input_file_path):

  balance = Stack()
  
  test = [symbol for row in text for symbol in row]
  
  for symbol in test:
  
    if symbol == "(":
    
      balance.push(symbol)
      
    elif symbol == ")":
    
      if balance.isEmpty():
      
        return False
        
      balance.pop()
      
  вызов функции сохранения в файл
  
  save_to_file("test4.txt", str(balance.isEmpty()))
  
   вернем True/False
   
  return balance.isEmpty()
  
 вызов функции чтения из файл
 
text = read_file("task4.txt")

balance_round_brackets(text)

### Задание №5

Дан текстовый файл с программой на алгоритмическом языке. За один просмотр файла проверить баланс квадратных скобок в тексте, используя дек.

def balance_square_brackets(input_file_path):

  balance = Deque()
  
  test = [symbol for row in text for symbol in row]
  
  for symbol in test:
  
    if symbol == "[":
    
      balance.append(symbol)
      
    elif symbol == "]":
    
      if balance.isEmpty():
      
        return False
        
      balance.pop()
      
 вызов функции сохранения в файл
 
  save_to_file("test5.txt", str(balance.isEmpty()))
  
   вернем True/False
   
  return balance.isEmpty()
  
 вызов функции чтения из файл
 
text = read_file("task5.txt")

balance_square_brackets(text)

### Задание №6

Дан файл из символов. Используя стек, за один просмотр файла напечатать сначала все цифры, затем все буквы, и, наконец, все остальные символы, сохраняя исходный порядок в каждой группе символов.

def sort_text(input_file_path):

  stack = Stack()
  
  new_doc = ""
  
  for word in text:
  
    for symbol in word:
    
     добавление в строку чисел иначе в стек
      
      if symbol.isdigit():
      
        new_doc += symbol
        
      else:
      
        stack.push(symbol)
        
     добавление в строку букв
    
    for item in stack.items:
    
      if item.isalpha():
      
        new_doc += item
        
     добавление в строку остальных символов
    
    for item in stack.items:
    
      if not item.isalpha():
      
        new_doc += item

   вызов функции сохранения в файл
  
  save_to_file("test6.txt", new_doc)
  
  return new_doc
  
 вызов функции чтения из файл

text = read_file("task6.txt")

sort_text(text)

### Задание №7
Дан файл из целых чисел. Используя дек, за один просмотр файла напечатать сначала все отрицательные числа, затем все положительные числа, сохраняя исходный порядок в каждой группе.

def sort_digits(input_file_path):

  deq = Deque()
  
  digits = [int(number.strip()) for number in numbers]
  
  [deq.append(num) for num in digits if num >= 0]
  
  digits.reverse()
  
  [deq.appendleft(num) for num in digits if num < 0]
  
   вызов функции сохранения в файл
   
  save_to_file("test7.txt", deq.items)
  
  while not deq.isEmpty():
  
    print(deq.popleft())
    
 вызов функции чтения из файл
 
numbers = read_file("task7.txt")

sort_digits(numbers)

### Задание №8

Дан текстовый файл. Используя стек, сформировать новый текстовый файл, содержащий строки исходного файла, записанные в обратном порядке: первая строка становится последней, вторая – предпоследней и т.д.

def reverse_strings(input_file_path):

    phrases = Stack()
    
    [phrases.push(string) for string in strings]
    
    phrases.reverse()
    
    # вызов функции сохранения в файл
    
    save_to_file("test8.txt", phrases.items)
    
    phrases.reverse()
    
    # вывод на экран
    
    while not phrases.isEmpty():
    
        print(phrases.pop())
        
 вызов функции чтения из файл

strings = read_file("task8.txt")

reverse_strings(strings)

### Вывод

Разработал программы обработки данных, содержащихся в заранее подготовленном txt-файле, в соответствии с заданиями, применив указанную в задании структуру данных. Результат работы программ вывел на экран и сохранил в отдельном txt-файле.


