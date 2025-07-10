# Алгебраические структуры

## 🎯 Цель курса

Изучение алгебраических структур (групп, колец, полей) и их применений в программировании, криптографии и дискретной математике.

## 📚 Содержание

1. [Основные понятия](#основные-понятия)
2. [Группы](#группы)
3. [Кольца](#кольца)
4. [Поля](#поля)
5. [Применения в программировании](#применения-в-программировании)
6. [Конечные поля](#конечные-поля)

---

## Основные понятия

### Бинарная операция

**Определение:**
Бинарная операция $*$ на множестве $S$ - это функция $* : S \times S \to S$.

**Свойства операций:**

**Ассоциативность:**
$$\forall a, b, c \in S: (a * b) * c = a * (b * c)$$

**Коммутативность:**
$$\forall a, b \in S: a * b = b * a$$

**Существование нейтрального элемента:**
$$\exists e \in S: \forall a \in S: a * e = e * a = a$$

**Существование обратного элемента:**
$$\forall a \in S: \exists a^{-1} \in S: a * a^{-1} = a^{-1} * a = e$$

### Магма, полугруппа, моноид

**Магма (группоид):**
$(S, *)$ - множество с бинарной операцией.

**Полугруппа:**
$(S, *)$ - магма с ассоциативной операцией.

**Моноид:**
$(S, *, e)$ - полугруппа с нейтральным элементом.

```python
class Monoid:
    def __init__(self, elements, operation, identity):
        self.elements = elements
        self.operation = operation
        self.identity = identity
    
    def verify_associativity(self):
        """Проверка ассоциативности"""
        for a in self.elements:
            for b in self.elements:
                for c in self.elements:
                    if self.operation(self.operation(a, b), c) != \
                       self.operation(a, self.operation(b, c)):
                        return False
        return True
    
    def verify_identity(self):
        """Проверка нейтрального элемента"""
        for a in self.elements:
            if self.operation(a, self.identity) != a or \
               self.operation(self.identity, a) != a:
                return False
        return True

# Пример: моноид строк с конкатенацией
string_monoid = Monoid(
    elements=["", "a", "b", "ab", "ba", "aa", "bb"],
    operation=lambda x, y: x + y,
    identity=""
)
```

---

## Группы

### Определение

**Группа** $(G, *)$ - это множество $G$ с бинарной операцией $*$, удовлетворяющей:
1. **Замкнутость**: $\forall a, b \in G: a * b \in G$
2. **Ассоциативность**: $\forall a, b, c \in G: (a * b) * c = a * (b * c)$
3. **Нейтральный элемент**: $\exists e \in G: \forall a \in G: a * e = e * a = a$
4. **Обратный элемент**: $\forall a \in G: \exists a^{-1} \in G: a * a^{-1} = a^{-1} * a = e$

### Свойства групп

**Единственность нейтрального элемента:**
$$\text{Если } e_1, e_2 \text{ - нейтральные элементы, то } e_1 = e_2$$

**Единственность обратного элемента:**
$$\text{Для каждого } a \in G \text{ существует единственный } a^{-1}$$

**Закон сокращения:**
$$ab = ac \Rightarrow b = c$$
$$ba = ca \Rightarrow b = c$$

### Примеры групп

**Аддитивная группа целых чисел:**
$$(\mathbb{Z}, +, 0)$$

**Мультипликативная группа ненулевых рациональных чисел:**
$$(\mathbb{Q}^*, \cdot, 1)$$

**Группа подстановок (симметрическая группа):**
$$S_n = \{\sigma: \{1, 2, \ldots, n\} \to \{1, 2, \ldots, n\} \mid \sigma \text{ биекция}\}$$

### Циклические группы

**Определение:**
Группа $G$ называется циклической, если $\exists g \in G$ такой, что:
$$G = \langle g \rangle = \{g^n \mid n \in \mathbb{Z}\}$$

**Порядок элемента:**
$$\text{ord}(g) = \min\{n \in \mathbb{N} \mid g^n = e\}$$

**Теорема Лагранжа:**
Для конечной группы $G$ и подгруппы $H \leq G$:
$$|H| \text{ делит } |G|$$

```python
class Group:
    def __init__(self, elements, operation, identity):
        self.elements = elements
        self.operation = operation
        self.identity = identity
    
    def inverse(self, element):
        """Нахождение обратного элемента"""
        for candidate in self.elements:
            if (self.operation(element, candidate) == self.identity and
                self.operation(candidate, element) == self.identity):
                return candidate
        return None
    
    def order(self, element):
        """Порядок элемента"""
        current = element
        order = 1
        
        while current != self.identity:
            current = self.operation(current, element)
            order += 1
            
            if order > len(self.elements):
                return float('inf')  # Бесконечный порядок
        
        return order
    
    def generate_cyclic_subgroup(self, generator):
        """Генерация циклической подгруппы"""
        subgroup = {self.identity}
        current = generator
        
        while current not in subgroup:
            subgroup.add(current)
            current = self.operation(current, generator)
        
        return subgroup

# Пример: группа Z/5Z
def mod_add(a, b, n=5):
    return (a + b) % n

z5_group = Group(
    elements={0, 1, 2, 3, 4},
    operation=lambda a, b: mod_add(a, b, 5),
    identity=0
)
```

### Гомоморфизмы

**Гомоморфизм групп:**
$$\phi: (G, *) \to (H, \circ)$$
$$\forall a, b \in G: \phi(a * b) = \phi(a) \circ \phi(b)$$

**Изоморфизм:**
Биективный гомоморфизм.

**Автоморфизм:**
Изоморфизм группы на себя.

**Ядро и образ:**
$$\ker(\phi) = \{g \in G \mid \phi(g) = e_H\}$$
$$\text{Im}(\phi) = \{\phi(g) \mid g \in G\}$$

**Первая теорема об изоморфизме:**
$$G / \ker(\phi) \cong \text{Im}(\phi)$$

---

## Кольца

### Определение

**Кольцо** $(R, +, \cdot)$ - это множество $R$ с двумя бинарными операциями, удовлетворяющими:
1. $(R, +)$ - абелева группа
2. $(R, \cdot)$ - полугруппа
3. **Дистрибутивность**: 
   $$a \cdot (b + c) = a \cdot b + a \cdot c$$
   $$(a + b) \cdot c = a \cdot c + b \cdot c$$

### Типы колец

**Коммутативное кольцо:**
$$\forall a, b \in R: a \cdot b = b \cdot a$$

**Кольцо с единицей:**
$$\exists 1 \in R: \forall a \in R: a \cdot 1 = 1 \cdot a = a$$

**Область целостности:**
Коммутативное кольцо с единицей без делителей нуля:
$$a \cdot b = 0 \Rightarrow a = 0 \text{ или } b = 0$$

### Примеры колец

**Кольцо целых чисел:**
$$(\mathbb{Z}, +, \cdot)$$

**Кольцо многочленов:**
$$R[x] = \{a_0 + a_1x + a_2x^2 + \ldots + a_nx^n \mid a_i \in R\}$$

**Кольцо матриц:**
$$M_n(R) = \{A \mid A \text{ - матрица } n \times n \text{ над } R\}$$

### Идеалы

**Левый идеал:**
$$I \subseteq R \text{ такой, что } \forall a \in I, r \in R: ra \in I$$

**Правый идеал:**
$$I \subseteq R \text{ такой, что } \forall a \in I, r \in R: ar \in I$$

**Двусторонний идеал:**
Одновременно левый и правый идеал.

**Главный идеал:**
$$\langle a \rangle = \{ra + as + \sum_{i} r_i a s_i \mid r, s, r_i, s_i \in R\}$$

### Факторкольца

**Факторкольцо:**
$$R/I = \{r + I \mid r \in R\}$$

**Операции:**
$$(r + I) + (s + I) = (r + s) + I$$
$$(r + I) \cdot (s + I) = (rs) + I$$

```python
class Ring:
    def __init__(self, elements, add, mult, zero, one=None):
        self.elements = elements
        self.add = add
        self.mult = mult
        self.zero = zero
        self.one = one
    
    def is_commutative(self):
        """Проверка коммутативности умножения"""
        for a in self.elements:
            for b in self.elements:
                if self.mult(a, b) != self.mult(b, a):
                    return False
        return True
    
    def has_zero_divisors(self):
        """Проверка на делители нуля"""
        for a in self.elements:
            if a == self.zero:
                continue
            for b in self.elements:
                if b == self.zero:
                    continue
                if self.mult(a, b) == self.zero:
                    return True
        return False
    
    def is_field(self):
        """Проверка, является ли кольцо полем"""
        if not self.is_commutative():
            return False
        
        # Каждый ненулевой элемент должен иметь обратный
        for a in self.elements:
            if a == self.zero:
                continue
            
            has_inverse = False
            for b in self.elements:
                if self.mult(a, b) == self.one:
                    has_inverse = True
                    break
            
            if not has_inverse:
                return False
        
        return True

# Пример: кольцо Z/6Z
def mod_add(a, b, n=6):
    return (a + b) % n

def mod_mult(a, b, n=6):
    return (a * b) % n

z6_ring = Ring(
    elements={0, 1, 2, 3, 4, 5},
    add=lambda a, b: mod_add(a, b, 6),
    mult=lambda a, b: mod_mult(a, b, 6),
    zero=0,
    one=1
)
```

---

## Поля

### Определение

**Поле** $(F, +, \cdot)$ - это коммутативное кольцо с единицей, в котором каждый ненулевой элемент обратим:
$$\forall a \in F, a \neq 0: \exists a^{-1} \in F: a \cdot a^{-1} = 1$$

### Свойства полей

**Характеристика поля:**
$$\text{char}(F) = \min\{n \in \mathbb{N} \mid n \cdot 1 = 0\}$$

Если такого $n$ не существует, то $\text{char}(F) = 0$.

**Основные свойства:**
- Характеристика поля либо 0, либо простое число
- Каждое конечное поле имеет $p^n$ элементов для некоторого простого $p$

### Примеры полей

**Поле рациональных чисел:**
$$(\mathbb{Q}, +, \cdot)$$

**Поле действительных чисел:**
$$(\mathbb{R}, +, \cdot)$$

**Поле комплексных чисел:**
$$(\mathbb{C}, +, \cdot)$$

**Конечное поле:**
$$\mathbb{F}_p = \mathbb{Z}/p\mathbb{Z} \text{ для простого } p$$

### Расширения полей

**Простое расширение:**
$$F(\alpha) = \{f(\alpha) \mid f \in F[x]\}$$

**Алгебраическое расширение:**
$\alpha$ алгебраичен над $F$, если $\exists f \in F[x], f \neq 0: f(\alpha) = 0$.

**Минимальный многочлен:**
Многочлен наименьшей степени, для которого $\alpha$ является корнем.

```python
def extended_gcd(a, b):
    """Расширенный алгоритм Евклида"""
    if a == 0:
        return b, 0, 1
    
    gcd, x1, y1 = extended_gcd(b % a, a)
    x = y1 - (b // a) * x1
    y = x1
    
    return gcd, x, y

def mod_inverse(a, m):
    """Нахождение обратного элемента по модулю"""
    gcd, x, _ = extended_gcd(a, m)
    
    if gcd != 1:
        return None  # Обратного элемента не существует
    
    return x % m

class FiniteField:
    def __init__(self, p):
        """Конечное поле F_p"""
        self.p = p
        self.elements = list(range(p))
    
    def add(self, a, b):
        return (a + b) % self.p
    
    def mult(self, a, b):
        return (a * b) % self.p
    
    def inverse(self, a):
        if a == 0:
            return None
        return mod_inverse(a, self.p)
    
    def power(self, a, n):
        """Быстрое возведение в степень"""
        result = 1
        a = a % self.p
        
        while n > 0:
            if n % 2 == 1:
                result = (result * a) % self.p
            n = n // 2
            a = (a * a) % self.p
        
        return result

# Пример: поле F_7
F7 = FiniteField(7)
print(f"3 + 5 = {F7.add(3, 5)}")  # 1
print(f"3 * 5 = {F7.mult(3, 5)}")  # 1
print(f"3^(-1) = {F7.inverse(3)}")  # 5
```

---

## Применения в программировании

### Коды исправления ошибок

**Коды Рида-Соломона:**
Используют конечные поля для кодирования информации.

**Многочлен кодирования:**
$$p(x) = \sum_{i=0}^{k-1} m_i x^i$$

**Кодовое слово:**
$$c = (p(\alpha^0), p(\alpha^1), \ldots, p(\alpha^{n-1}))$$

где $\alpha$ - примитивный элемент поля.

### Криптография

**Эллиптические кривые:**
$$y^2 = x^3 + ax + b$$

**Сложение точек:**
Для точек $P = (x_1, y_1)$ и $Q = (x_2, y_2)$:

Если $P \neq Q$:
$$\lambda = \frac{y_2 - y_1}{x_2 - x_1}$$

Если $P = Q$:
$$\lambda = \frac{3x_1^2 + a}{2y_1}$$

**Результат:**
$$P + Q = (x_3, y_3)$$
$$x_3 = \lambda^2 - x_1 - x_2$$
$$y_3 = \lambda(x_1 - x_3) - y_1$$

### Хеширование

**Полиномиальное хеширование:**
$$h(s) = \sum_{i=0}^{n-1} s[i] \cdot p^i \bmod m$$

где $p$ - простое число, $m$ - модуль.

```python
def polynomial_hash(s, p=31, m=10**9 + 7):
    """Полиномиальное хеширование"""
    hash_value = 0
    p_power = 1
    
    for char in s:
        hash_value = (hash_value + ord(char) * p_power) % m
        p_power = (p_power * p) % m
    
    return hash_value

def rolling_hash(s, pattern_length, p=31, m=10**9 + 7):
    """Скользящее хеширование для поиска подстроки"""
    if len(s) < pattern_length:
        return []
    
    # Вычисляем p^(pattern_length-1) mod m
    p_power = pow(p, pattern_length - 1, m)
    
    # Хеш первого окна
    current_hash = 0
    for i in range(pattern_length):
        current_hash = (current_hash + ord(s[i]) * pow(p, i, m)) % m
    
    hashes = [current_hash]
    
    # Скользящее окно
    for i in range(pattern_length, len(s)):
        # Убираем левый символ
        current_hash = (current_hash - ord(s[i - pattern_length])) % m
        current_hash = (current_hash * pow(p, -1, m)) % m
        
        # Добавляем правый символ
        current_hash = (current_hash + ord(s[i]) * p_power) % m
        
        hashes.append(current_hash)
    
    return hashes
```

---

## Конечные поля

### Построение конечных полей

**Поле $\mathbb{F}_{p^n}$:**
Конструируется как $\mathbb{F}_p[x] / \langle f(x) \rangle$, где $f(x)$ - неприводимый многочлен степени $n$.

**Примитивные элементы:**
Элемент $\alpha$ называется примитивным, если $\text{ord}(\alpha) = p^n - 1$.

### Алгоритмы

**Проверка неприводимости:**
Многочлен $f(x)$ степени $n$ неприводим над $\mathbb{F}_p$, если:
$$\gcd(f(x), x^{p^i} - x) = 1 \text{ для всех } i = 1, 2, \ldots, \lfloor n/2 \rfloor$$

**Нахождение корней:**
Алгоритм Кантора-Цассенхауза для факторизации многочленов.

```python
class GaloisField:
    def __init__(self, p, irreducible_poly):
        """Поле Галуа GF(p^n)"""
        self.p = p
        self.irreducible_poly = irreducible_poly
        self.degree = len(irreducible_poly) - 1
    
    def add(self, a, b):
        """Сложение многочленов"""
        result = [0] * max(len(a), len(b))
        
        for i in range(len(a)):
            result[i] = (result[i] + a[i]) % self.p
        
        for i in range(len(b)):
            result[i] = (result[i] + b[i]) % self.p
        
        return self.normalize(result)
    
    def mult(self, a, b):
        """Умножение многочленов с приведением"""
        # Обычное умножение многочленов
        result = [0] * (len(a) + len(b) - 1)
        
        for i in range(len(a)):
            for j in range(len(b)):
                result[i + j] = (result[i + j] + a[i] * b[j]) % self.p
        
        # Приведение по неприводимому многочлену
        return self.reduce(result)
    
    def reduce(self, poly):
        """Приведение по неприводимому многочлену"""
        while len(poly) >= len(self.irreducible_poly):
            if poly[-1] == 0:
                poly.pop()
                continue
            
            # Деление на старший коэффициент
            coeff = poly[-1]
            
            # Вычитание кратного неприводимого многочлена
            for i in range(len(self.irreducible_poly)):
                idx = len(poly) - len(self.irreducible_poly) + i
                if idx >= 0:
                    poly[idx] = (poly[idx] - coeff * self.irreducible_poly[i]) % self.p
            
            # Убираем старший коэффициент
            poly.pop()
        
        return self.normalize(poly)
    
    def normalize(self, poly):
        """Убираем ведущие нули"""
        while len(poly) > 1 and poly[-1] == 0:
            poly.pop()
        return poly if poly else [0]

# Пример: GF(2^3) с неприводимым многочленом x^3 + x + 1
gf8 = GaloisField(2, [1, 1, 0, 1])  # x^3 + x + 1

# Элементы поля как многочлены
a = [1, 0, 1]  # x^2 + 1
b = [0, 1, 1]  # x^2 + x

print(f"a + b = {gf8.add(a, b)}")
print(f"a * b = {gf8.mult(a, b)}")
```

---

## Связанные темы

- [[number-theory]] - Теория чисел
- [[discrete-mathematics]] - Дискретная математика
- [[cryptography]] - Криптография
- [[linear-algebra]] - Линейная алгебра

---

*Алгебраические структуры обеспечивают фундаментальную основу для многих областей математики и информатики, включая криптографию, кодирование, компьютерную алгебру и формальную верификацию.* 