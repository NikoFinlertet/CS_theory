# Алгоритмы

> **Основные алгоритмы для разработчиков**
>
> Сортировка, поиск, рекурсия и динамическое программирование

## 📋 Классификация алгоритмов

### По типу задач
```
🔍 ПОИСК
├── Линейный поиск O(n)
├── Бинарный поиск O(log n)
└── Поиск в хэш-таблице O(1)

📊 СОРТИРОВКА
├── Простые: Bubble O(n²), Selection O(n²)
├── Эффективные: Merge O(n log n), Quick O(n log n)
└── Специальные: Counting O(n+k), Radix O(d×n)

🔄 РЕКУРСИЯ
├── Divide & Conquer
├── Backtracking
└── Dynamic Programming
```

---

## 🔍 Алгоритмы поиска

### Бинарный поиск

```go
// ==================== БИНАРНЫЙ ПОИСК ====================
// Временная сложность: O(log n)
// Пространственная сложность: O(1)
func binarySearch(arr []int, target int) int {
    left, right := 0, len(arr)-1
    
    for left <= right {
        // Избегаем переполнения при (left + right) / 2
        mid := left + (right-left)/2
        
        if arr[mid] == target {
            return mid  // Найден!
        } else if arr[mid] < target {
            left = mid + 1  // Ищем в правой половине
        } else {
            right = mid - 1  // Ищем в левой половине
        }
    }
    
    return -1  // Не найден
}

// ==================== ПОИСК ГРАНИЦ ====================
// Найти первое и последнее вхождение элемента
func searchRange(nums []int, target int) []int {
    left := findFirst(nums, target)
    if left == -1 {
        return []int{-1, -1}
    }
    
    right := findLast(nums, target)
    return []int{left, right}
}

func findFirst(nums []int, target int) int {
    left, right := 0, len(nums)-1
    result := -1
    
    for left <= right {
        mid := left + (right-left)/2
        
        if nums[mid] == target {
            result = mid      // Запоминаем позицию
            right = mid - 1   // Продолжаем поиск влево
        } else if nums[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    return result
}
```

---

## 📊 Алгоритмы сортировки

### Merge Sort (Сортировка слиянием)

```python
def merge_sort(arr):
    """
    Сортировка слиянием - стабильная сортировка
    Временная сложность: O(n log n) во всех случаях
    Пространственная сложность: O(n)
    """
    if len(arr) <= 1:
        return arr
    
    # Делим массив пополам
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])    # Рекурсивно сортируем левую часть
    right = merge_sort(arr[mid:])   # Рекурсивно сортируем правую часть
    
    # Сливаем отсортированные части
    return merge(left, right)

def merge(left, right):
    """
    Слияние двух отсортированных массивов
    Временная сложность: O(n)
    """
    result = []
    i = j = 0
    
    # Сравниваем элементы и добавляем меньший
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    # Добавляем оставшиеся элементы
    result.extend(left[i:])
    result.extend(right[j:])
    
    return result
```

### Quick Sort (Быстрая сортировка)

```go
// ==================== БЫСТРАЯ СОРТИРОВКА ====================
// Временная сложность: O(n log n) в среднем, O(n²) в худшем случае
// Пространственная сложность: O(log n) - стек рекурсии
func quickSort(arr []int, low, high int) {
    if low < high {
        // Разделяем массив и получаем позицию опорного элемента
        pi := partition(arr, low, high)
        
        // Рекурсивно сортируем части до и после опорного элемента
        quickSort(arr, low, pi-1)
        quickSort(arr, pi+1, high)
    }
}

func partition(arr []int, low, high int) int {
    // Выбираем последний элемент как опорный (pivot)
    pivot := arr[high]
    
    // Индекс меньшего элемента - указывает на правильную позицию pivot
    i := low - 1
    
    for j := low; j < high; j++ {
        // Если текущий элемент меньше или равен опорному
        if arr[j] <= pivot {
            i++
            arr[i], arr[j] = arr[j], arr[i]  // Меняем местами
        }
    }
    
    // Ставим опорный элемент на правильную позицию
    arr[i+1], arr[high] = arr[high], arr[i+1]
    
    return i + 1
}

// ==================== УЛУЧШЕННАЯ ВЕРСИЯ С СЛУЧАЙНЫМ PIVOT ====================
func quickSortRandomized(arr []int, low, high int) {
    if low < high {
        // Выбираем случайный pivot для избежания худшего случая
        randomPivot := low + rand.Intn(high-low+1)
        arr[randomPivot], arr[high] = arr[high], arr[randomPivot]
        
        pi := partition(arr, low, high)
        quickSortRandomized(arr, low, pi-1)
        quickSortRandomized(arr, pi+1, high)
    }
}
```

---

## 🔄 Динамическое программирование

### Числа Фибоначчи

```python
# ==================== НАИВНАЯ РЕКУРСИЯ O(2^n) ====================
def fibonacci_naive(n):
    """Неэффективная реализация с экспоненциальной сложностью"""
    if n <= 1:
        return n
    return fibonacci_naive(n-1) + fibonacci_naive(n-2)

# ==================== МЕМОИЗАЦИЯ O(n) ====================
def fibonacci_memo(n, memo={}):
    """Мемоизация - запоминание результатов"""
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    # Вычисляем и запоминаем результат
    memo[n] = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo)
    return memo[n]

# ==================== ТАБУЛЯЦИЯ O(n) ====================
def fibonacci_dp(n):
    """Bottom-up подход с массивом"""
    if n <= 1:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

# ==================== ОПТИМИЗИРОВАННАЯ ВЕРСИЯ O(n) время, O(1) память ====================
def fibonacci_optimized(n):
    """Используем только две переменные вместо массива"""
    if n <= 1:
        return n
    
    prev2, prev1 = 0, 1
    
    for i in range(2, n + 1):
        current = prev1 + prev2
        prev2, prev1 = prev1, current
    
    return prev1
```

### Задача о рюкзаке

```go
// ==================== ЗАДАЧА О РЮКЗАКЕ 0/1 ====================
// Временная сложность: O(n * W), где n - количество предметов, W - вместимость
// Пространственная сложность: O(n * W)

func knapsack(weights []int, values []int, capacity int) int {
    n := len(weights)
    
    // dp[i][w] = максимальная стоимость с использованием первых i предметов
    // и вместимостью w
    dp := make([][]int, n+1)
    for i := range dp {
        dp[i] = make([]int, capacity+1)
    }
    
    // Заполняем таблицу снизу вверх
    for i := 1; i <= n; i++ {
        for w := 0; w <= capacity; w++ {
            // Если предмет не помещается
            if weights[i-1] > w {
                dp[i][w] = dp[i-1][w]  // Не берем предмет
            } else {
                // Выбираем максимум между:
                // 1. Не берем предмет: dp[i-1][w]
                // 2. Берем предмет: values[i-1] + dp[i-1][w-weights[i-1]]
                take := values[i-1] + dp[i-1][w-weights[i-1]]
                notTake := dp[i-1][w]
                dp[i][w] = max(take, notTake)
            }
        }
    }
    
    return dp[n][capacity]
}

// ==================== ОПТИМИЗИРОВАННАЯ ВЕРСИЯ O(W) ПАМЯТИ ====================
func knapsackOptimized(weights []int, values []int, capacity int) int {
    n := len(weights)
    
    // Используем только один массив вместо 2D таблицы
    dp := make([]int, capacity+1)
    
    for i := 0; i < n; i++ {
        // Идем справа налево чтобы не перезаписать нужные значения
        for w := capacity; w >= weights[i]; w-- {
            dp[w] = max(dp[w], dp[w-weights[i]]+values[i])
        }
    }
    
    return dp[capacity]
}
```

---

## 🔢 Математические алгоритмы

### Алгоритм Евклида (НОД)

```python
def gcd(a, b):
    """
    Наибольший общий делитель - алгоритм Евклида
    Временная сложность: O(log(min(a, b)))
    """
    while b:
        a, b = b, a % b
    return a

def lcm(a, b):
    """
    Наименьшее общее кратное
    НОК(a, b) = (a * b) / НОД(a, b)
    """
    return abs(a * b) // gcd(a, b)

# ==================== РАСШИРЕННЫЙ АЛГОРИТМ ЕВКЛИДА ====================
def extended_gcd(a, b):
    """
    Расширенный алгоритм Евклида
    Находит НОД и коэффициенты x, y такие что ax + by = НОД(a, b)
    """
    if a == 0:
        return b, 0, 1
    
    gcd_val, x1, y1 = extended_gcd(b % a, a)
    
    x = y1 - (b // a) * x1
    y = x1
    
    return gcd_val, x, y
```

### Быстрое возведение в степень

```go
// ==================== БЫСТРОЕ ВОЗВЕДЕНИЕ В СТЕПЕНЬ ====================
// Временная сложность: O(log n)
// Пространственная сложность: O(1)
func fastPower(base, exponent int) int {
    result := 1
    
    for exponent > 0 {
        // Если степень нечетная, умножаем на base
        if exponent%2 == 1 {
            result *= base
        }
        
        // Возводим base в квадрат и делим степень пополам
        base *= base
        exponent /= 2
    }
    
    return result
}

// ==================== С МОДУЛЕМ (для больших чисел) ====================
func fastPowerMod(base, exponent, mod int) int {
    result := 1
    base = base % mod
    
    for exponent > 0 {
        if exponent%2 == 1 {
            result = (result * base) % mod
        }
        
        exponent = exponent >> 1  // Деление на 2 через битовый сдвиг
        base = (base * base) % mod
    }
    
    return result
}
```

---

## 🧮 Алгоритмы на строках

### Поиск подстроки (KMP)

```python
def kmp_search(text, pattern):
    """
    Поиск подстроки алгоритмом Кнута-Морриса-Пратта
    Временная сложность: O(n + m)
    где n - длина текста, m - длина паттерна
    """
    def compute_lps(pattern):
        """Вычисляем массив LPS (Longest Proper Prefix Suffix)"""
        m = len(pattern)
        lps = [0] * m
        length = 0
        i = 1
        
        while i < m:
            if pattern[i] == pattern[length]:
                length += 1
                lps[i] = length
                i += 1
            else:
                if length != 0:
                    length = lps[length - 1]
                else:
                    lps[i] = 0
                    i += 1
        
        return lps
    
    n = len(text)
    m = len(pattern)
    
    if m == 0:
        return []
    
    lps = compute_lps(pattern)
    matches = []
    
    i = 0  # Индекс для text
    j = 0  # Индекс для pattern
    
    while i < n:
        if pattern[j] == text[i]:
            i += 1
            j += 1
        
        if j == m:
            matches.append(i - j)  # Найдено совпадение
            j = lps[j - 1]
        elif i < n and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    
    return matches
```

---

## 🌐 Алгоритмы на графах

### Поиск в ширину (BFS)

```go
// ==================== BFS ДЛЯ ПОИСКА КРАТЧАЙШЕГО ПУТИ ====================
func bfs(graph [][]int, start, end int) []int {
    if start == end {
        return []int{start}
    }
    
    visited := make([]bool, len(graph))
    queue := []int{start}
    parent := make([]int, len(graph))
    
    // Инициализируем parent значениями -1
    for i := range parent {
        parent[i] = -1
    }
    
    visited[start] = true
    
    for len(queue) > 0 {
        current := queue[0]
        queue = queue[1:]  // Удаляем из начала очереди
        
        // Проверяем всех соседей
        for _, neighbor := range graph[current] {
            if !visited[neighbor] {
                visited[neighbor] = true
                parent[neighbor] = current
                queue = append(queue, neighbor)
                
                // Если достигли цели, восстанавливаем путь
                if neighbor == end {
                    return reconstructPath(parent, start, end)
                }
            }
        }
    }
    
    return nil  // Путь не найден
}

func reconstructPath(parent []int, start, end int) []int {
    path := []int{}
    current := end
    
    // Идем от конца к началу через parent
    for current != -1 {
        path = append([]int{current}, path...)  // Добавляем в начало
        current = parent[current]
    }
    
    return path
}
```

---

## 🔍 Алгоритмы поиска и оптимизации

### Тернарный поиск

```python
def ternary_search(arr, left, right, target):
    """
    Тернарный поиск для унимодальных функций
    Временная сложность: O(log₃ n)
    """
    if right >= left:
        # Делим на три части
        mid1 = left + (right - left) // 3
        mid2 = right - (right - left) // 3
        
        if arr[mid1] == target:
            return mid1
        if arr[mid2] == target:
            return mid2
        
        # Определяем, в какой трети искать
        if target < arr[mid1]:
            return ternary_search(arr, left, mid1 - 1, target)
        elif target > arr[mid2]:
            return ternary_search(arr, mid2 + 1, right, target)
        else:
            return ternary_search(arr, mid1 + 1, mid2 - 1, target)
    
    return -1

# ==================== ПОИСК МАКСИМУМА УНИМОДАЛЬНОЙ ФУНКЦИИ ====================
def find_maximum_ternary(func, left, right, precision=1e-9):
    """
    Поиск максимума унимодальной функции тернарным поиском
    """
    while right - left > precision:
        m1 = left + (right - left) / 3
        m2 = right - (right - left) / 3
        
        if func(m1) < func(m2):
            left = m1
        else:
            right = m2
    
    return (left + right) / 2
```

---

## 📊 Сравнение алгоритмов

| Алгоритм | Лучший случай | Средний случай | Худший случай | Память |
|----------|---------------|----------------|---------------|---------|
| **Поиск** |
| Линейный | O(1) | O(n) | O(n) | O(1) |
| Бинарный | O(1) | O(log n) | O(log n) | O(1) |
| **Сортировка** |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |

---

## 🎯 Советы по выбору алгоритма

### Для поиска
- **Неупорядоченные данные:** Линейный поиск O(n)
- **Упорядоченные данные:** Бинарный поиск O(log n)
- **Частые поиски:** Хэш-таблица O(1)

### Для сортировки
- **Малые данные (< 50 элементов):** Insertion Sort
- **Средние данные:** Quick Sort
- **Большие данные:** Merge Sort
- **Стабильность важна:** Merge Sort
- **Минимум памяти:** Heap Sort

### Для строк
- **Один поиск:** Встроенные функции
- **Множественные поиски:** KMP, Rabin-Karp
- **Поиск по шаблону:** Регулярные выражения

---

*Хороший алгоритм — это не самый умный, а самый подходящий для задачи* 🎯 