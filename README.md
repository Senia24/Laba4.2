# Лабораторная работа №4: Структуры данных
## Задание:
Задание: Словарь.

Дано слово и словарь. Необходимо найти все слова из словаря, которые можно составить из букв данного слова, и вывести их в порядке уменьшения длины.

В качестве словаря можно использовать любой текстовый файл с любыми словами, но желательно использовать словарь со словами русского языка.

Ограничение времени на решение 2 с.

Допустимо использовать больше времени (до 1 мин) на этап инициализации или обработки словаря. Но после этого этапа время на обработку нового слова не более 2 с.

Список слов (словарь) можно взять тут https://github.com/Harrix/Russian-Nouns/releases

## Реализация:

### Листинг программы:
``` python
import sys
import time
class AnagramFinder:
    def __init__(self, dictionary_file):
        self.words_by_length = {}
        self.word_freqs = {}
        
        try:
            with open(dictionary_file, 'r', encoding='utf-8') as f:
                for line in f:
                    word = line.strip().lower()
                    if word and word.isalpha():  # только буквенные слова
                        length = len(word)
                        if length not in self.words_by_length:
                            self.words_by_length[length] = []
                        self.words_by_length[length].append(word)
                        
                        # Ручной подсчёт частот
                        freq = {}
                        for ch in word:
                            freq[ch] = freq.get(ch, 0) + 1
                        self.word_freqs[word] = freq
        except FileNotFoundError:
            print(f"Ошибка: файл '{dictionary_file}' не найден!")
            sys.exit(1)
    
    def get_word_freq(self, word):
        # Подсчёт частоты букв в слове
        freq = {}
        for ch in word:
            freq[ch] = freq.get(ch, 0) + 1
        return freq
    
    def can_form(self, source_freq, target_freq):
        # Проверка возможности составить слово
        for ch, cnt in target_freq.items():
            if source_freq.get(ch, 0) < cnt:
                return False
        return True
    
    def find_words(self, source_word):
        source_word = source_word.lower().strip()
        source_freq = self.get_word_freq(source_word)
        
        result = []
        source_len = len(source_word)
        
        # Перебираем длины от 1 до длины исходного слова
        for length in range(1, source_len + 1):
            if length in self.words_by_length:
                for word in self.words_by_length[length]:
                    if self.can_form(source_freq, self.word_freqs[word]):
                        result.append(word)
        
        # Сортировка по убыванию длины (при равной длине - по алфавиту)
        result.sort(key=lambda x: (-len(x), x))
        return result


if len(sys.argv) > 1:
    dict_file = sys.argv[1]
else:
    dict_file = "Russian_nouns.txt"  
    print(f"Используется словарь по умолчанию: {dict_file}")
    
finder = AnagramFinder(dict_file)
    
while True:
    word = input("Введите слово (или 'q' для выхода): ").strip()
        
    if word.lower() == 'q':
        break
        
    if not word:
        continue
    start_time = time.time()    
    results = finder.find_words(word)
    print(f"Всего найдено: {len(results)}")
    if results:    
        for w in results:
            print(f"{w}")
    else:
        print("нет слов")
    end_time = time.time()
    print(f"Затрачено времени: {end_time-start_time:.5f} сек")
    print("Автор: Кочаров Арсений Андреевич, группа:090301-ПОВа-о25")    
```
## Результат выполнения программы:
<img width="532" height="302" alt="image" src="https://github.com/user-attachments/assets/0c9a9453-ca12-416d-8d3e-9cdd06b53259" />
