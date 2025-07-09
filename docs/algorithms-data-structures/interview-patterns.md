# Паттерны решения задач

> **Основные паттерны для успешного прохождения технических интервью**
>
> От Two Pointers до Backtracking с реальными примерами задач

## 🎯 Зачем нужны паттерны?

**Паттерны помогают:**
- 🧠 Распознавать типы задач за секунды
- ⚡ Быстро находить эффективные решения
- 🔄 Применять знакомые подходы к новым задачам
- 📝 Структурированно решать задачи на интервью

---

## 👉 Two Pointers (Два указателя)

### Когда использовать
- Массив/строка отсортированы
- Ищем пару элементов с определенным условием
- Нужно найти подмассив/подстроку

### Шаблон решения

```python
def two_pointers_template(arr):
    """
    Базовый шаблон для двух указателей
    """
    left = 0
    right = len(arr) - 1
    
    while left < right:
        # Проверяем условие
        if condition_met(arr[left], arr[right]):
            # Обрабатываем найденную пару
            process_pair(arr[left], arr[right])
            left += 1
            right -= 1
        elif need_larger_sum(arr[left], arr[right]):
            left += 1  # Увеличиваем левый указатель
        else:
            right -= 1  # Уменьшаем правый указатель
    
    return result
```

### Пример 1: Two Sum в отсортированном массиве

```go
// ==================== TWO SUM В ОТСОРТИРОВАННОМ МАССИВЕ ====================
// Временная сложность: O(n)
// Пространственная сложность: O(1)
func twoSum(numbers []int, target int) []int {
    left := 0
    right := len(numbers) - 1
    
    for left < right {
        currentSum := numbers[left] + numbers[right]
        
        if currentSum == target {
            // Найдена пара! Возвращаем индексы (1-based)
            return []int{left + 1, right + 1}
        } else if currentSum < target {
            // Нужна большая сумма - двигаем левый указатель
            left++
        } else {
            // Нужна меньшая сумма - двигаем правый указатель
            right--
        }
    }
    
    return []int{}  // Не найдено
}
```

### Пример 2: Удаление дубликатов

```python
def remove_duplicates(arr):
    """
    Удаление дубликатов из отсортированного массива
    Временная сложность: O(n)
    Пространственная сложность: O(1)
    """
    if not arr:
        return 0
    
    # Два указателя: slow - для записи, fast - для чтения
    slow = 0
    
    for fast in range(1, len(arr)):
        # Если элементы разные, записываем в позицию slow
        if arr[fast] != arr[slow]:
            slow += 1
            arr[slow] = arr[fast]
    
    return slow + 1  # Новая длина массива
```

---

## 🔄 Sliding Window (Скользящее окно)

### Когда использовать
- Нужно найти подмассив/подстроку фиксированной длины
- Максимум/минимум в подмассиве
- Подмассив с определенным условием

### Шаблон решения

```python
def sliding_window_template(arr, k):
    """
    Базовый шаблон скользящего окна
    """
    left = 0
    window_sum = 0
    result = []
    
    for right in range(len(arr)):
        # Расширяем окно
        window_sum += arr[right]
        
        # Если окно достигло нужного размера
        while window_size_condition(left, right, k):
            # Обрабатываем текущее окно
            process_window(arr, left, right, window_sum)
            
            # Сжимаем окно
            window_sum -= arr[left]
            left += 1
    
    return result
```

### Пример 1: Максимальная сумма подмассива длины k

```go
// ==================== МАКСИМАЛЬНАЯ СУММА ПОДМАССИВА ДЛИНЫ K ====================
// Временная сложность: O(n)
// Пространственная сложность: O(1)
func maxSumSubarray(arr []int, k int) int {
    if len(arr) < k {
        return 0
    }
    
    // Вычисляем сумму первого окна
    windowSum := 0
    for i := 0; i < k; i++ {
        windowSum += arr[i]
    }
    
    maxSum := windowSum
    
    // Скользим окно по оставшимся элементам
    for i := k; i < len(arr); i++ {
        // Убираем левый элемент, добавляем правый
        windowSum = windowSum - arr[i-k] + arr[i]
        maxSum = max(maxSum, windowSum)
    }
    
    return maxSum
}
```

### Пример 2: Длинейшая подстрока без повторений

```python
def longest_substring_without_repeating(s):
    """
    Длинейшая подстрока без повторяющихся символов
    Временная сложность: O(n)
    Пространственная сложность: O(min(m, n)) где m - размер алфавита
    """
    char_set = set()
    left = 0
    max_length = 0
    
    for right in range(len(s)):
        # Если символ уже есть в окне, сжимаем окно
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        # Добавляем новый символ
        char_set.add(s[right])
        
        # Обновляем максимальную длину
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

---

## 🏃 Fast & Slow Pointers (Быстрый и медленный указатели)

### Когда использовать
- Поиск цикла в связном списке
- Нахождение середины списка
- Определение пересечения списков

### Пример: Поиск цикла (Floyd's Algorithm)

```go
// ==================== ПОИСК ЦИКЛА В СВЯЗНОМ СПИСКЕ ====================
// Временная сложность: O(n)
// Пространственная сложность: O(1)

type ListNode struct {
    Val  int
    Next *ListNode
}

func hasCycle(head *ListNode) bool {
    if head == nil || head.Next == nil {
        return false
    }
    
    // Медленный указатель движется на 1 шаг
    slow := head
    // Быстрый указатель движется на 2 шага
    fast := head.Next
    
    // Если есть цикл, быстрый указатель догонит медленный
    for slow != fast {
        // Быстрый указатель дошел до конца - нет цикла
        if fast == nil || fast.Next == nil {
            return false
        }
        
        slow = slow.Next      // +1 шаг
        fast = fast.Next.Next // +2 шага
    }
    
    return true  // Указатели встретились - есть цикл
}

// ==================== НАХОЖДЕНИЕ НАЧАЛА ЦИКЛА ====================
func detectCycle(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return nil
    }
    
    // Фаза 1: Поиск встречи указателей
    slow := head
    fast := head
    
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
        
        if slow == fast {
            break  // Найдена встреча
        }
    }
    
    // Цикла нет
    if fast == nil || fast.Next == nil {
        return nil
    }
    
    // Фаза 2: Поиск начала цикла
    // Один указатель в начале, другой в точке встречи
    slow = head
    for slow != fast {
        slow = slow.Next
        fast = fast.Next
    }
    
    return slow  // Начало цикла
}
```

---

## 🌳 Tree Traversal (Обход деревьев)

### DFS (Depth-First Search)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# ==================== РЕКУРСИВНЫЙ DFS ====================
def dfs_recursive(root, result):
    """
    Рекурсивный обход дерева в глубину
    Временная сложность: O(n)
    Пространственная сложность: O(h) где h - высота дерева
    """
    if not root:
        return
    
    # Preorder: корень -> левое -> правое
    result.append(root.val)
    dfs_recursive(root.left, result)
    dfs_recursive(root.right, result)

# ==================== ИТЕРАТИВНЫЙ DFS ====================
def dfs_iterative(root):
    """
    Итеративный обход дерева с использованием стека
    """
    if not root:
        return []
    
    result = []
    stack = [root]
    
    while stack:
        node = stack.pop()
        result.append(node.val)
        
        # Добавляем в стек справа налево (для left-first обхода)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    
    return result
```

### BFS (Breadth-First Search)

```python
from collections import deque

def bfs_level_order(root):
    """
    Обход дерева по уровням (BFS)
    Временная сложность: O(n)
    Пространственная сложность: O(w) где w - максимальная ширина дерева
    """
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        # Обрабатываем все узлы текущего уровня
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.val)
            
            # Добавляем детей в очередь
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(current_level)
    
    return result
```

---

## 🎭 Backtracking (Возврат)

### Когда использовать
- Генерация всех возможных комбинаций
- Поиск всех решений
- Задачи с ограничениями

### Шаблон решения

```python
def backtrack_template(candidates, target, current_combination, result):
    """
    Базовый шаблон backtracking
    """
    # Базовый случай: найдено решение
    if is_valid_solution(current_combination, target):
        result.append(current_combination[:])  # Копируем решение
        return
    
    # Перебираем все возможные варианты
    for candidate in candidates:
        # Проверяем, можно ли добавить кандидата
        if is_valid_choice(candidate, current_combination):
            # Добавляем кандидата
            current_combination.append(candidate)
            
            # Рекурсивно ищем дальше
            backtrack_template(candidates, target, current_combination, result)
            
            # Откатываем изменения (backtrack)
            current_combination.pop()
```

### Пример: Генерация всех подмножеств

```python
def subsets(nums):
    """
    Генерация всех возможных подмножеств
    Временная сложность: O(2^n)
    Пространственная сложность: O(n) - глубина рекурсии
    """
    result = []
    
    def backtrack(start, current_subset):
        # Добавляем текущее подмножество в результат
        result.append(current_subset[:])
        
        # Генерируем подмножества, добавляя элементы
        for i in range(start, len(nums)):
            # Добавляем элемент
            current_subset.append(nums[i])
            
            # Рекурсивно генерируем подмножества
            backtrack(i + 1, current_subset)
            
            # Убираем элемент (backtrack)
            current_subset.pop()
    
    backtrack(0, [])
    return result
```

---

## 🔍 Binary Search Pattern (Бинарный поиск)

### Расширенные применения

```python
def binary_search_pattern(arr, target):
    """
    Обобщенный паттерн бинарного поиска
    """
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if condition(arr[mid], target):
            return mid  # Или обновить результат
        elif should_search_left(arr[mid], target):
            right = mid - 1
        else:
            left = mid + 1
    
    return -1  # Не найдено

# ==================== ПОИСК В ПОВЕРНУТОМ МАССИВЕ ====================
def search_rotated_array(nums, target):
    """
    Поиск в повернутом отсортированном массиве
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        
        # Левая половина отсортирована
        if nums[left] <= nums[mid]:
            # Цель в левой половине
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Правая половина отсортирована
        else:
            # Цель в правой половине
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1
```

---

## 🏗️ Merge Intervals (Слияние интервалов)

### Когда использовать
- Работа с временными интервалами
- Объединение пересекающихся диапазонов
- Поиск свободных слотов

### Пример: Слияние интервалов

```go
// ==================== СЛИЯНИЕ ПЕРЕСЕКАЮЩИХСЯ ИНТЕРВАЛОВ ====================
// Временная сложность: O(n log n) - сортировка
// Пространственная сложность: O(n) - результат

type Interval struct {
    Start int
    End   int
}

func merge(intervals []Interval) []Interval {
    if len(intervals) <= 1 {
        return intervals
    }
    
    // Сортируем интервалы по начальной точке
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i].Start < intervals[j].Start
    })
    
    merged := []Interval{intervals[0]}
    
    for i := 1; i < len(intervals); i++ {
        current := intervals[i]
        last := &merged[len(merged)-1]
        
        // Если интервалы пересекаются
        if current.Start <= last.End {
            // Объединяем интервалы
            last.End = max(last.End, current.End)
        } else {
            // Добавляем новый интервал
            merged = append(merged, current)
        }
    }
    
    return merged
}
```

---

## 🔄 Cyclic Sort (Циклическая сортировка)

### Когда использовать
- Массив содержит числа в диапазоне [1, n]
- Нужно найти пропущенные/дублирующиеся числа
- Требуется O(1) дополнительная память

### Пример: Найти пропущенные числа

```python
def find_missing_numbers(nums):
    """
    Поиск всех пропущенных чисел в диапазоне [1, n]
    Временная сложность: O(n)
    Пространственная сложность: O(1)
    """
    i = 0
    n = len(nums)
    
    # Циклическая сортировка: помещаем каждое число на свое место
    while i < n:
        # Правильная позиция для числа nums[i] это nums[i] - 1
        correct_pos = nums[i] - 1
        
        # Если число не на своем месте, меняем местами
        if nums[i] != nums[correct_pos]:
            nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        else:
            i += 1
    
    # Находим пропущенные числа
    missing = []
    for i in range(n):
        if nums[i] != i + 1:
            missing.append(i + 1)
    
    return missing
```

---

## 💡 Dynamic Programming Patterns

### Основные паттерны DP

```python
# ==================== ПАТТЕРН 1: 0/1 KNAPSACK ====================
def knapsack_pattern(weights, values, capacity):
    """
    Паттерн рюкзака: выбор элементов с максимальной выгодой
    """
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(capacity + 1):
            if weights[i-1] <= w:
                # Выбираем максимум: брать или не брать элемент
                dp[i][w] = max(
                    dp[i-1][w],  # Не берем
                    dp[i-1][w-weights[i-1]] + values[i-1]  # Берем
                )
            else:
                dp[i][w] = dp[i-1][w]  # Элемент не помещается
    
    return dp[n][capacity]

# ==================== ПАТТЕРН 2: LONGEST COMMON SUBSEQUENCE ====================
def lcs_pattern(text1, text2):
    """
    Паттерн наибольшей общей подпоследовательности
    """
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1  # Символы совпадают
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])  # Берем лучший вариант
    
    return dp[m][n]
```

---

## 🎯 Как применять паттерны на интервью

### Пошаговый подход

1. **Понимание задачи (2 минуты)**
   - Прочитать условие дважды
   - Задать уточняющие вопросы
   - Разобрать примеры

2. **Выбор паттерна (1 минута)**
   - Отсортированный массив → Two Pointers/Binary Search
   - Подмассив/подстрока → Sliding Window
   - Связный список + цикл → Fast & Slow Pointers
   - Дерево → DFS/BFS
   - Все комбинации → Backtracking

3. **Объяснение подхода (2 минуты)**
   - Назвать паттерн
   - Объяснить логику
   - Оценить сложность

4. **Реализация (15-20 минут)**
   - Начать с шаблона
   - Адаптировать под задачу
   - Тестировать на примерах

5. **Оптимизация (5 минут)**
   - Можно ли улучшить по времени?
   - Можно ли улучшить по памяти?
   - Краевые случаи

---

## 🏆 Топ-10 паттернов для FAANG

1. **Two Pointers** - 15% задач
2. **Sliding Window** - 12% задач
3. **Binary Search** - 10% задач
4. **DFS/BFS** - 15% задач
5. **Dynamic Programming** - 20% задач
6. **Backtracking** - 8% задач
7. **Greedy** - 10% задач
8. **Merge Intervals** - 5% задач
9. **Cyclic Sort** - 3% задач
10. **Topological Sort** - 2% задач

---

*Паттерны - это мышцы разума. Чем больше тренируешься, тем быстрее распознаешь решения* 🧠💪 