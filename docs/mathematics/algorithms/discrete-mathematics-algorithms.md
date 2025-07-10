# Дискретная математика в алгоритмах 🔢

> **Навигация**: [[mathematics-algorithms|← Главная]] | [[graph-theory-structures|Теория графов →]]

## Содержание
1. [Теория множеств и операции с битами](#🔢-теория-множеств-и-операции-с-битами)
2. [Рекуррентные соотношения и динамическое программирование](#📊-рекуррентные-соотношения-и-динамическое-программирование)
3. [Практические применения](#🚀-практические-применения)

---

## 🔢 Теория множеств и операции с битами

**Математическая основа:** Множества, булева алгебра, операции пересечения, объединения, дополнения

**Алгоритмические применения:**
- Битовые маски для представления подмножеств
- Операции с битами для оптимизации
- Фильтры Блума

**Когда использовать:**
- ✅ Работа с флагами и состояниями
- ✅ Оптимизация памяти
- ✅ Быстрые операции над множествами

```python
class BitSetOperations:
    """Операции с множествами через битовые маски"""
    
    def __init__(self, max_size=32):
        self.bits = 0
        self.max_size = max_size
    
    def add(self, element):
        """Добавить элемент в множество"""
        if 0 <= element < self.max_size:
            self.bits |= (1 << element)
    
    def remove(self, element):
        """Удалить элемент из множества"""
        if 0 <= element < self.max_size:
            self.bits &= ~(1 << element)
    
    def contains(self, element):
        """Проверить принадлежность элемента множеству"""
        if 0 <= element < self.max_size:
            return bool(self.bits & (1 << element))
        return False
    
    def union(self, other):
        """Объединение множеств"""
        result = BitSetOperations(self.max_size)
        result.bits = self.bits | other.bits
        return result
    
    def intersection(self, other):
        """Пересечение множеств"""
        result = BitSetOperations(self.max_size)
        result.bits = self.bits & other.bits
        return result
    
    def complement(self):
        """Дополнение множества"""
        result = BitSetOperations(self.max_size)
        result.bits = ~self.bits & ((1 << self.max_size) - 1)
        return result
    
    def subset_count(self):
        """Количество подмножеств (2^n)"""
        return 1 << bin(self.bits).count('1')

# Пример применения в задаче о рюкзаке
def knapsack_bitmask(weights, values, capacity):
    """Решение задачи о рюкзаке через битовые маски"""
    n = len(weights)
    max_value = 0
    
    # Перебираем все возможные подмножества (2^n)
    for mask in range(1 << n):
        current_weight = 0
        current_value = 0
        
        # Проверяем каждый элемент в подмножестве
        for i in range(n):
            if mask & (1 << i):  # Если i-й элемент включен
                current_weight += weights[i]
                current_value += values[i]
        
        if current_weight <= capacity:
            max_value = max(max_value, current_value)
    
    return max_value
```

---

## 📊 Рекуррентные соотношения и динамическое программирование

**Математическая основа:** Рекуррентные уравнения, принцип оптимальности Беллмана

**Алгоритмические применения:**
- Динамическое программирование
- Мемоизация
- Разделяй и властвуй

**Когда использовать:**
- ✅ Задачи с оптимальной подструктурой
- ✅ Перекрывающиеся подзадачи
- ✅ Оптимизационные задачи

```python
class DynamicProgramming:
    """Примеры применения DP основанных на рекуррентных соотношениях"""
    
    @staticmethod
    def fibonacci_dp(n):
        """
        Fibonacci: F(n) = F(n-1) + F(n-2), F(0)=0, F(1)=1
        Время: O(n), Память: O(n) → O(1) с оптимизацией
        """
        if n <= 1:
            return n
        
        # Оптимизированная версия с O(1) памятью
        a, b = 0, 1
        for _ in range(2, n + 1):
            a, b = b, a + b
        return b
    
    @staticmethod
    def catalan_numbers(n):
        """
        Числа Каталана: C(n) = (1/(n+1)) * C(2n, n)
        Рекуррентно: C(n) = Σ(i=0 to n-1) C(i) * C(n-1-i)
        Применение: количество правильных скобочных последовательностей
        """
        if n <= 1:
            return 1
        
        dp = [0] * (n + 1)
        dp[0], dp[1] = 1, 1
        
        for i in range(2, n + 1):
            for j in range(i):
                dp[i] += dp[j] * dp[i - 1 - j]
        
        return dp[n]
    
    @staticmethod
    def edit_distance(str1, str2):
        """
        Расстояние Левенштейна
        DP[i][j] = min(
            DP[i-1][j] + 1,      # deletion
            DP[i][j-1] + 1,      # insertion
            DP[i-1][j-1] + cost  # substitution
        )
        """
        m, n = len(str1), len(str2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        
        # Инициализация базовых случаев
        for i in range(m + 1):
            dp[i][0] = i
        for j in range(n + 1):
            dp[0][j] = j
        
        # Заполнение таблицы
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                cost = 0 if str1[i-1] == str2[j-1] else 1
                dp[i][j] = min(
                    dp[i-1][j] + 1,      # deletion
                    dp[i][j-1] + 1,      # insertion
                    dp[i-1][j-1] + cost  # substitution
                )
        
        return dp[m][n]
    
    @staticmethod
    def longest_common_subsequence(text1, text2):
        """
        Наибольшая общая подпоследовательность
        LCS[i][j] = LCS[i-1][j-1] + 1 если text1[i-1] == text2[j-1]
                   иначе max(LCS[i-1][j], LCS[i][j-1])
        """
        m, n = len(text1), len(text2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        
        return dp[m][n]
```

---

## 🚀 Практические применения

### Битовые маски в решении задач
```python
def subset_sum_bitmask(numbers, target):
    """Проверка существования подмножества с заданной суммой"""
    n = len(numbers)
    
    for mask in range(1 << n):
        current_sum = 0
        for i in range(n):
            if mask & (1 << i):
                current_sum += numbers[i]
        
        if current_sum == target:
            return True
    
    return False

def traveling_salesman_bitmask(distances):
    """TSP через битовые маски и DP"""
    n = len(distances)
    
    # dp[mask][i] = минимальная стоимость посетить все города в mask, заканчивая в i
    dp = [[float('inf')] * n for _ in range(1 << n)]
    dp[1][0] = 0  # Начинаем с города 0
    
    for mask in range(1 << n):
        for u in range(n):
            if not (mask & (1 << u)):
                continue
            
            for v in range(n):
                if mask & (1 << v):
                    continue
                
                new_mask = mask | (1 << v)
                dp[new_mask][v] = min(dp[new_mask][v], 
                                    dp[mask][u] + distances[u][v])
    
    # Возвращаемся в начальный город
    full_mask = (1 << n) - 1
    min_cost = float('inf')
    for i in range(1, n):
        min_cost = min(min_cost, dp[full_mask][i] + distances[i][0])
    
    return min_cost
```

### Применения DP в алгоритмах
```python
def matrix_chain_multiplication(dimensions):
    """
    Оптимальная скобочная расстановка при умножении цепочки матриц
    DP[i][j] = минимальное количество скалярных умножений для матриц i..j
    """
    n = len(dimensions) - 1
    dp = [[0] * n for _ in range(n)]
    
    # l - длина цепочки
    for l in range(2, n + 1):
        for i in range(n - l + 1):
            j = i + l - 1
            dp[i][j] = float('inf')
            
            for k in range(i, j):
                cost = (dp[i][k] + dp[k+1][j] + 
                       dimensions[i] * dimensions[k+1] * dimensions[j+1])
                dp[i][j] = min(dp[i][j], cost)
    
    return dp[0][n-1]

def coin_change_combinations(coins, amount):
    """
    Количество способов набрать сумму amount используя монеты coins
    DP[i] = количество способов набрать сумму i
    """
    dp = [0] * (amount + 1)
    dp[0] = 1  # Один способ набрать 0 - не брать монет
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] += dp[i - coin]
    
    return dp[amount]
```

---

## 🔗 Связанные темы

- [[graph-theory-structures|Теория графов и структуры данных]] - применение графов в алгоритмах
- [[linear-algebra-computation|Линейная алгебра в вычислениях]] - матричные алгоритмы  
- [[probability-randomized-algorithms|Вероятностные алгоритмы]] - рандомизированные методы
- [[combinatorics-generation|Комбинаторика и генерация]] - перечислительные алгоритмы

## 📚 Дополнительные ресурсы

- **Книги**: "Concrete Mathematics" - Graham, Knuth, Patashnik
- **Курсы**: MIT 6.042J Mathematics for Computer Science
- **Практика**: LeetCode Dynamic Programming, Bit Manipulation sections 