# Теория информации

## 🎯 Цель курса

Изучение математических основ информации, кодирования и передачи данных с применением в сжатии данных, криптографии и машинном обучении.

## 📚 Содержание

1. [Основные понятия](#основные-понятия)
2. [Энтропия и информация](#энтропия-и-информация)
3. [Кодирование источников](#кодирование-источников)
4. [Канальное кодирование](#канальное-кодирование)
5. [Взаимная информация](#взаимная-информация)
6. [Применения в ML](#применения-в-ml)

---

## Основные понятия

### Количество информации

**Количество информации в событии:**
$$I(x) = -\log_2 P(x)$$

где $P(x)$ - вероятность события $x$.

**Единицы измерения:**
- **Бит (bit)** - логарифм по основанию 2
- **Нат (nat)** - логарифм по основанию $e$
- **Дит (dit)** - логарифм по основанию 10

### Энтропия

**Энтропия дискретного источника:**
$$H(X) = -\sum_{i=1}^{n} p_i \log_2 p_i$$

**Свойства энтропии:**
- $H(X) \geq 0$ (неотрицательность)
- $H(X) = 0$ тогда и только тогда, когда $X$ детерминирован
- $H(X) \leq \log_2 n$ (максимальная энтропия для равномерного распределения)

### Практическая реализация

```python
import math
import numpy as np
from collections import Counter

def entropy(data):
    """Вычисление энтропии для дискретных данных"""
    if not data:
        return 0
    
    # Подсчет частот
    counts = Counter(data)
    probabilities = [count / len(data) for count in counts.values()]
    
    # Вычисление энтропии
    entropy = 0
    for p in probabilities:
        if p > 0:
            entropy -= p * math.log2(p)
    
    return entropy

def information_content(probability):
    """Количество информации в событии"""
    if probability <= 0:
        return float('inf')
    return -math.log2(probability)

# Пример использования
data = ['A', 'B', 'A', 'C', 'A', 'B', 'A']
print(f"Энтропия: {entropy(data):.3f} бит")
print(f"Информация в событии 'A': {information_content(4/7):.3f} бит")
```

---

## Энтропия и информация

### Условная энтропия

**Условная энтропия:**
$$H(Y|X) = \sum_{x} P(x) H(Y|X=x)$$

$$H(Y|X) = -\sum_{x,y} P(x,y) \log_2 P(y|x)$$

**Цепное правило:**
$$H(X,Y) = H(X) + H(Y|X)$$

### Относительная энтропия

**Дивергенция Кульбака-Лейблера:**
$$D_{KL}(P||Q) = \sum_{x} P(x) \log_2 \frac{P(x)}{Q(x)}$$

**Свойства:**
- $D_{KL}(P||Q) \geq 0$
- $D_{KL}(P||Q) = 0$ тогда и только тогда, когда $P = Q$

### Кросс-энтропия

**Кросс-энтропия:**
$$H(P,Q) = -\sum_{x} P(x) \log_2 Q(x)$$

**Связь с KL-дивергенцией:**
$$H(P,Q) = H(P) + D_{KL}(P||Q)$$

```python
def conditional_entropy(X, Y):
    """Условная энтропия H(Y|X)"""
    joint_counts = Counter(zip(X, Y))
    x_counts = Counter(X)
    
    conditional_entropy = 0
    for (x, y), joint_count in joint_counts.items():
        p_xy = joint_count / len(X)
        p_x = x_counts[x] / len(X)
        p_y_given_x = joint_count / x_counts[x]
        
        conditional_entropy -= p_xy * math.log2(p_y_given_x)
    
    return conditional_entropy

def kl_divergence(P, Q):
    """KL-дивергенция между двумя распределениями"""
    divergence = 0
    for p, q in zip(P, Q):
        if p > 0 and q > 0:
            divergence += p * math.log2(p / q)
    return divergence

def cross_entropy(P, Q):
    """Кросс-энтропия между двумя распределениями"""
    cross_ent = 0
    for p, q in zip(P, Q):
        if p > 0 and q > 0:
            cross_ent -= p * math.log2(q)
    return cross_ent
```

---

## Кодирование источников

### Коды с переменной длиной

**Неравенство Крафта:**
Для префиксного кода с длинами кодовых слов $l_1, l_2, \ldots, l_n$:
$$\sum_{i=1}^{n} 2^{-l_i} \leq 1$$

**Теорема кодирования источника:**
Для любого источника с энтропией $H(X)$ и любого $\epsilon > 0$ существует код, такой что:
$$H(X) \leq \bar{L} < H(X) + \epsilon$$

где $\bar{L}$ - средняя длина кодового слова.

### Код Хаффмана

**Алгоритм Хаффмана:**
1. Создать листы для всех символов с их частотами
2. Построить дерево снизу вверх, объединяя узлы с минимальными частотами
3. Назначить коды: 0 для левых ветвей, 1 для правых

```python
import heapq
from collections import defaultdict

class Node:
    def __init__(self, freq, symbol=None, left=None, right=None):
        self.freq = freq
        self.symbol = symbol
        self.left = left
        self.right = right
    
    def __lt__(self, other):
        return self.freq < other.freq

def huffman_coding(frequencies):
    """Построение кода Хаффмана"""
    if len(frequencies) == 1:
        return {list(frequencies.keys())[0]: '0'}
    
    # Создание кучи из узлов
    heap = [Node(freq, symbol) for symbol, freq in frequencies.items()]
    heapq.heapify(heap)
    
    # Построение дерева
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        
        merged = Node(left.freq + right.freq, left=left, right=right)
        heapq.heappush(heap, merged)
    
    # Извлечение кодов
    root = heap[0]
    codes = {}
    
    def extract_codes(node, code=''):
        if node.symbol is not None:
            codes[node.symbol] = code
        else:
            extract_codes(node.left, code + '0')
            extract_codes(node.right, code + '1')
    
    extract_codes(root)
    return codes

# Пример использования
frequencies = {'A': 45, 'B': 13, 'C': 12, 'D': 16, 'E': 9, 'F': 5}
codes = huffman_coding(frequencies)
print("Коды Хаффмана:")
for symbol, code in codes.items():
    print(f"{symbol}: {code}")
```

### Арифметическое кодирование

**Принцип:**
Кодирование последовательности символов как одного числа в интервале $[0, 1)$.

**Алгоритм:**
1. Начальный интервал: $[0, 1)$
2. Для каждого символа разделить интервал пропорционально вероятностям
3. Выбрать подинтервал, соответствующий символу
4. Повторить для следующего символа

```python
def arithmetic_encode(message, probabilities):
    """Арифметическое кодирование"""
    # Построение кумулятивных вероятностей
    cumulative = {}
    cum_prob = 0
    for symbol in sorted(probabilities.keys()):
        cumulative[symbol] = cum_prob
        cum_prob += probabilities[symbol]
    
    # Кодирование
    low = 0.0
    high = 1.0
    
    for symbol in message:
        range_size = high - low
        high = low + range_size * (cumulative[symbol] + probabilities[symbol])
        low = low + range_size * cumulative[symbol]
    
    # Возврат любого числа в финальном интервале
    return (low + high) / 2

def arithmetic_decode(code, probabilities, length):
    """Арифметическое декодирование"""
    # Построение кумулятивных вероятностей
    cumulative = {}
    cum_prob = 0
    for symbol in sorted(probabilities.keys()):
        cumulative[symbol] = cum_prob
        cum_prob += probabilities[symbol]
    
    # Декодирование
    decoded = []
    low = 0.0
    high = 1.0
    
    for _ in range(length):
        range_size = high - low
        scaled_value = (code - low) / range_size
        
        # Найти символ
        for symbol in sorted(probabilities.keys()):
            if cumulative[symbol] <= scaled_value < cumulative[symbol] + probabilities[symbol]:
                decoded.append(symbol)
                # Обновить интервал
                high = low + range_size * (cumulative[symbol] + probabilities[symbol])
                low = low + range_size * cumulative[symbol]
                break
    
    return ''.join(decoded)
```

---

## Канальное кодирование

### Пропускная способность канала

**Пропускная способность:**
$$C = \max_{P(X)} I(X;Y)$$

где $I(X;Y)$ - взаимная информация между входом и выходом канала.

**Для двоичного симметричного канала:**
$$C = 1 - H(p)$$

где $p$ - вероятность ошибки, $H(p) = -p \log_2 p - (1-p) \log_2 (1-p)$.

### Коды для исправления ошибок

**Расстояние Хемминга:**
$$d(x,y) = \sum_{i=1}^{n} \mathbf{1}_{x_i \neq y_i}$$

**Минимальное расстояние кода:**
$$d_{min} = \min_{x \neq y} d(x,y)$$

**Граница Хемминга:**
$$M \leq \frac{2^n}{\sum_{i=0}^{t} \binom{n}{i}}$$

где $M$ - число кодовых слов, $t$ - число исправляемых ошибок.

```python
def hamming_distance(x, y):
    """Расстояние Хемминга между двумя строками"""
    return sum(c1 != c2 for c1, c2 in zip(x, y))

def generate_hamming_7_4():
    """Генерация кода Хемминга (7,4)"""
    # Порождающая матрица
    G = np.array([
        [1, 0, 0, 0, 1, 1, 0],
        [0, 1, 0, 0, 1, 0, 1],
        [0, 0, 1, 0, 0, 1, 1],
        [0, 0, 0, 1, 1, 1, 1]
    ])
    
    # Проверочная матрица
    H = np.array([
        [1, 1, 0, 1, 1, 0, 0],
        [1, 0, 1, 1, 0, 1, 0],
        [0, 1, 1, 1, 0, 0, 1]
    ])
    
    return G, H

def hamming_encode(data, G):
    """Кодирование с помощью кода Хемминга"""
    return np.dot(data, G) % 2

def hamming_decode(received, H):
    """Декодирование с исправлением одиночных ошибок"""
    syndrome = np.dot(H, received) % 2
    
    if np.sum(syndrome) == 0:
        # Нет ошибок
        return received[:4]  # Информационные биты
    else:
        # Найти позицию ошибки
        error_pos = 0
        for i in range(3):
            if syndrome[i] == 1:
                error_pos += 2**i
        
        # Исправить ошибку
        corrected = received.copy()
        corrected[error_pos - 1] = 1 - corrected[error_pos - 1]
        
        return corrected[:4]
```

---

## Взаимная информация

### Определение

**Взаимная информация:**
$$I(X;Y) = \sum_{x,y} P(x,y) \log_2 \frac{P(x,y)}{P(x)P(y)}$$

**Альтернативные формы:**
$$I(X;Y) = H(X) - H(X|Y)$$
$$I(X;Y) = H(Y) - H(Y|X)$$
$$I(X;Y) = H(X) + H(Y) - H(X,Y)$$

### Свойства

**Основные свойства:**
- $I(X;Y) = I(Y;X)$ (симметричность)
- $I(X;Y) \geq 0$ (неотрицательность)
- $I(X;X) = H(X)$ (автоинформация)
- $I(X;Y) = 0$ тогда и только тогда, когда $X$ и $Y$ независимы

```python
def mutual_information(X, Y):
    """Вычисление взаимной информации"""
    # Подсчет совместных и маргинальных частот
    joint_counts = Counter(zip(X, Y))
    x_counts = Counter(X)
    y_counts = Counter(Y)
    
    n = len(X)
    mi = 0
    
    for (x, y), joint_count in joint_counts.items():
        p_xy = joint_count / n
        p_x = x_counts[x] / n
        p_y = y_counts[y] / n
        
        mi += p_xy * math.log2(p_xy / (p_x * p_y))
    
    return mi

def normalized_mutual_information(X, Y):
    """Нормализованная взаимная информация"""
    mi = mutual_information(X, Y)
    h_x = entropy(X)
    h_y = entropy(Y)
    
    return mi / math.sqrt(h_x * h_y)
```

---

## Применения в ML

### Выбор признаков

**Критерий взаимной информации:**
Выбираем признаки с максимальной взаимной информацией с целевой переменной.

```python
def feature_selection_mi(X, y, k=10):
    """Выбор k лучших признаков по взаимной информации"""
    scores = []
    
    for i in range(X.shape[1]):
        mi = mutual_information(X[:, i], y)
        scores.append((i, mi))
    
    # Сортировка по убыванию взаимной информации
    scores.sort(key=lambda x: x[1], reverse=True)
    
    # Возврат индексов k лучших признаков
    return [idx for idx, _ in scores[:k]]
```

### Оценка кластеризации

**Adjusted Mutual Information (AMI):**
$$AMI = \frac{MI - E[MI]}{\max(H(X), H(Y)) - E[MI]}$$

где $E[MI]$ - ожидаемая взаимная информация для случайного разбиения.

### Регуляризация в нейронных сетях

**Information Bottleneck:**
Минимизация функции:
$$\mathcal{L} = -I(T;Y) + \beta I(X;T)$$

где $T$ - скрытое представление, $\beta$ - параметр регуляризации.

---

## Продвинутые темы

### Алгоритмическая теория информации

**Колмогоровская сложность:**
$$K(x) = \min_{p} \{|p| : U(p) = x\}$$

где $U$ - универсальная машина Тьюринга.

### Сжатие данных

**Универсальные коды:**
- Код Элиаса-γ
- Код Элиаса-δ
- Код Фибоначчи

**Алгоритм Лемпеля-Зива:**
```python
def lz77_compress(data, window_size=4096):
    """Сжатие LZ77"""
    compressed = []
    i = 0
    
    while i < len(data):
        # Поиск самого длинного совпадения в окне
        best_match = (0, 0)  # (offset, length)
        
        start = max(0, i - window_size)
        for j in range(start, i):
            length = 0
            while (i + length < len(data) and 
                   j + length < i and 
                   data[j + length] == data[i + length]):
                length += 1
            
            if length > best_match[1]:
                best_match = (i - j, length)
        
        if best_match[1] > 0:
            # Найдено совпадение
            compressed.append(('match', best_match[0], best_match[1]))
            i += best_match[1]
        else:
            # Нет совпадения
            compressed.append(('literal', data[i]))
            i += 1
    
    return compressed
```

---

## Связанные темы

- [[probability-theory]] - Теория вероятностей
- [[cryptography-security]] - Криптография
- [[machine-learning-theory]] - Машинное обучение
- [[discrete-mathematics]] - Дискретная математика

---

*Теория информации является фундаментальной для понимания процессов передачи, хранения и обработки информации. Она находит применение в сжатии данных, криптографии, машинном обучении и многих других областях компьютерных наук.* 