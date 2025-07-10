# Математическая логика

## 🎯 Цель курса

Изучение формальных систем логики и их применения в программировании, верификации программ и искусственном интеллекте.

## 📚 Содержание

1. [Пропозициональная логика](#пропозициональная-логика)
2. [Логика предикатов](#логика-предикатов)
3. [Системы доказательств](#системы-доказательств)
4. [Теория моделей](#теория-моделей)
5. [Теория вычислимости](#теория-вычислимости)
6. [Приложения в программировании](#приложения-в-программировании)

---

## Пропозициональная логика

### Основные операции

**Основные логические связки:**
- **Отрицание**: $\neg p$
- **Конъюнкция**: $p \land q$
- **Дизъюнкция**: $p \lor q$
- **Импликация**: $p \to q$
- **Эквивалентность**: $p \leftrightarrow q$

**Производные операции:**
- **Исключающее ИЛИ**: $p \oplus q \equiv (p \lor q) \land \neg(p \land q)$
- **Штрих Шеффера**: $p \mid q \equiv \neg(p \land q)$
- **Стрелка Пирса**: $p \downarrow q \equiv \neg(p \lor q)$

### Семантика

**Таблицы истинности:**

| $p$ | $q$ | $\neg p$ | $p \land q$ | $p \lor q$ | $p \to q$ | $p \leftrightarrow q$ |
|-----|-----|----------|-------------|------------|-----------|----------------------|
| T   | T   | F        | T           | T          | T         | T                    |
| T   | F   | F        | F           | T          | F         | F                    |
| F   | T   | T        | F           | T          | T         | F                    |
| F   | F   | T        | F           | F          | T         | T                    |

**Тавтология**: $\models \varphi$ (формула истинна при любой интерпретации)

**Противоречие**: $\models \neg \varphi$ (формула ложна при любой интерпретации)

### Равносильные преобразования

**Законы де Моргана:**
$$\neg(p \land q) \equiv \neg p \lor \neg q$$
$$\neg(p \lor q) \equiv \neg p \land \neg q$$

**Дистрибутивность:**
$$p \land (q \lor r) \equiv (p \land q) \lor (p \land r)$$
$$p \lor (q \land r) \equiv (p \lor q) \land (p \lor r)$$

**Законы идемпотентности:**
$$p \land p \equiv p$$
$$p \lor p \equiv p$$

**Законы поглощения:**
$$p \land (p \lor q) \equiv p$$
$$p \lor (p \land q) \equiv p$$

### Нормальные формы

**Дизъюнктивная нормальная форма (ДНФ):**
$$\varphi \equiv \bigvee_{i=1}^{n} \left(\bigwedge_{j=1}^{k_i} l_{ij}\right)$$

где $l_{ij}$ - литерал (переменная или её отрицание).

**Конъюнктивная нормальная форма (КНФ):**
$$\varphi \equiv \bigwedge_{i=1}^{n} \left(\bigvee_{j=1}^{k_i} l_{ij}\right)$$

**Алгоритм преобразования в КНФ:**
1. Устранить импликации: $p \to q \equiv \neg p \lor q$
2. Переместить отрицания к переменным (законы де Моргана)
3. Применить дистрибутивность

```python
class PropositionalFormula:
    def __init__(self, formula):
        self.formula = formula
    
    def to_cnf(self):
        """Преобразование в КНФ"""
        # Устранение импликаций
        formula = self.eliminate_implications()
        
        # Перемещение отрицаний
        formula = self.move_negations(formula)
        
        # Дистрибутивность
        formula = self.distribute(formula)
        
        return formula
    
    def eliminate_implications(self):
        """Устранение импликаций"""
        # p -> q становится ~p | q
        # p <-> q становится (p -> q) & (q -> p)
        pass
    
    def move_negations(self, formula):
        """Перемещение отрицаний к переменным"""
        # Применение законов де Моргана
        pass
    
    def distribute(self, formula):
        """Применение дистрибутивности"""
        # A | (B & C) становится (A | B) & (A | C)
        pass

def is_tautology(formula):
    """Проверка, является ли формула тавтологией"""
    variables = extract_variables(formula)
    
    # Перебор всех возможных значений переменных
    for assignment in all_assignments(variables):
        if not evaluate(formula, assignment):
            return False
    
    return True

def satisfiable(formula):
    """Проверка выполнимости формулы"""
    variables = extract_variables(formula)
    
    for assignment in all_assignments(variables):
        if evaluate(formula, assignment):
            return True
    
    return False
```

---

## Логика предикатов

### Синтаксис

**Термы:**
- **Константы**: $a, b, c, \ldots$
- **Переменные**: $x, y, z, \ldots$
- **Функции**: $f(t_1, t_2, \ldots, t_n)$

**Атомарные формулы:**
$$P(t_1, t_2, \ldots, t_n)$$

**Формулы:**
- Атомарные формулы
- $\neg \varphi$, $\varphi \land \psi$, $\varphi \lor \psi$, $\varphi \to \psi$, $\varphi \leftrightarrow \psi$
- $\forall x \varphi$, $\exists x \varphi$

### Кванторы

**Универсальный квантор:**
$$\forall x P(x)$$
"Для всех $x$ выполняется $P(x)$"

**Экзистенциальный квантор:**
$$\exists x P(x)$$
"Существует $x$ такой, что $P(x)$"

**Связанные и свободные переменные:**
- В формуле $\forall x P(x, y)$ переменная $x$ связана, $y$ свободна
- $FV(\varphi)$ - множество свободных переменных в $\varphi$

### Законы кванторов

**Отрицание кванторов:**
$$\neg \forall x P(x) \equiv \exists x \neg P(x)$$
$$\neg \exists x P(x) \equiv \forall x \neg P(x)$$

**Дистрибутивность:**
$$\forall x (P(x) \land Q(x)) \equiv \forall x P(x) \land \forall x Q(x)$$
$$\exists x (P(x) \lor Q(x)) \equiv \exists x P(x) \lor \exists x Q(x)$$

**Коммутативность кванторов:**
$$\forall x \forall y P(x,y) \equiv \forall y \forall x P(x,y)$$
$$\exists x \exists y P(x,y) \equiv \exists y \exists x P(x,y)$$

**НЕ коммутативность разных кванторов:**
$$\forall x \exists y P(x,y) \not\equiv \exists y \forall x P(x,y)$$

### Пренексная нормальная форма

**Определение:**
$$Q_1 x_1 Q_2 x_2 \ldots Q_n x_n \varphi$$

где $Q_i \in \{\forall, \exists\}$ и $\varphi$ не содержит кванторов.

**Алгоритм приведения к ПНФ:**
1. Переименовать связанные переменные
2. Устранить импликации
3. Переместить отрицания к атомам
4. Вынести кванторы наружу

```python
def prenex_normal_form(formula):
    """Приведение к пренексной нормальной форме"""
    # Этап 1: Переименование переменных
    formula = rename_bound_variables(formula)
    
    # Этап 2: Устранение импликаций
    formula = eliminate_implications(formula)
    
    # Этап 3: Перемещение отрицаний
    formula = move_negations_to_atoms(formula)
    
    # Этап 4: Вынесение кванторов
    formula = extract_quantifiers(formula)
    
    return formula

def skolemize(formula):
    """Сколемизация - устранение экзистенциальных кванторов"""
    # Для каждого ∃x в области действия ∀y₁,...,∀yₙ
    # заменяем x на f(y₁,...,yₙ), где f - новая функция Сколема
    pass
```

---

## Системы доказательств

### Натуральная дедукция

**Правила введения:**

**Конъюнкция:**
$$\frac{\varphi \quad \psi}{\varphi \land \psi} \land I$$

**Дизъюнкция:**
$$\frac{\varphi}{\varphi \lor \psi} \lor I_1 \quad \frac{\psi}{\varphi \lor \psi} \lor I_2$$

**Импликация:**
$$\frac{[\varphi] \vdots \psi}{\varphi \to \psi} \to I$$

**Универсальный квантор:**
$$\frac{\varphi[x]}{\forall x \varphi} \forall I$$
(при условии, что $x$ не свободна в предположениях)

**Экзистенциальный квантор:**
$$\frac{\varphi[t]}{\exists x \varphi} \exists I$$

**Правила устранения:**

**Конъюнкция:**
$$\frac{\varphi \land \psi}{\varphi} \land E_1 \quad \frac{\varphi \land \psi}{\psi} \land E_2$$

**Дизъюнкция:**
$$\frac{\varphi \lor \psi \quad [\varphi] \vdots \chi \quad [\psi] \vdots \chi}{\chi} \lor E$$

**Импликация:**
$$\frac{\varphi \to \psi \quad \varphi}{\psi} \to E$$

**Универсальный квантор:**
$$\frac{\forall x \varphi}{\varphi[t]} \forall E$$

**Экзистенциальный квантор:**
$$\frac{\exists x \varphi \quad [\varphi[y]] \vdots \chi}{\chi} \exists E$$
(при условии, что $y$ не свободна в $\chi$ и предположениях)

### Резолюция

**Принцип резолюции:**
Из клозов $C_1 \lor A$ и $C_2 \lor \neg A$ выводим $C_1 \lor C_2$.

**Алгоритм резолюции:**
1. Привести формулу к КНФ
2. Представить как множество клозов
3. Применять резолюцию до получения пустого клоза

```python
def resolution(clauses):
    """Алгоритм резолюции"""
    new_clauses = set()
    
    while True:
        # Генерация всех возможных резольвент
        for c1 in clauses:
            for c2 in clauses:
                if c1 != c2:
                    resolvent = resolve(c1, c2)
                    if resolvent is not None:
                        if resolvent == set():  # Пустой клоз
                            return "Противоречие"
                        new_clauses.add(resolvent)
        
        if new_clauses.issubset(clauses):
            return "Выполнимо"
        
        clauses = clauses.union(new_clauses)

def resolve(clause1, clause2):
    """Резолюция двух клозов"""
    for literal in clause1:
        if negate(literal) in clause2:
            # Найден резолюируемый литерал
            new_clause = (clause1 - {literal}) | (clause2 - {negate(literal)})
            return new_clause
    
    return None
```

---

## Теория моделей

### Интерпретация

**Структура/Модель:**
$$\mathcal{M} = \langle D, I \rangle$$

где:
- $D$ - непустое множество (универсум)
- $I$ - функция интерпретации

**Интерпретация символов:**
- Константы: $I(c) \in D$
- Функции: $I(f): D^n \to D$
- Предикаты: $I(P) \subseteq D^n$

### Выполнимость

**Выполнимость формулы:**
$$\mathcal{M} \models \varphi[s]$$

где $s$ - оценка переменных.

**Рекурсивное определение:**
- $\mathcal{M} \models P(t_1, \ldots, t_n)[s]$ тогда и только тогда, когда $\langle \mathcal{M}[t_1][s], \ldots, \mathcal{M}[t_n][s] \rangle \in I(P)$
- $\mathcal{M} \models \neg \varphi[s]$ тогда и только тогда, когда $\mathcal{M} \not\models \varphi[s]$
- $\mathcal{M} \models \varphi \land \psi[s]$ тогда и только тогда, когда $\mathcal{M} \models \varphi[s]$ и $\mathcal{M} \models \psi[s]$
- $\mathcal{M} \models \forall x \varphi[s]$ тогда и только тогда, когда для всех $d \in D$ выполняется $\mathcal{M} \models \varphi[s[x \mapsto d]]$

### Теоремы полноты и корректности

**Теорема корректности:**
Если $\Gamma \vdash \varphi$, то $\Gamma \models \varphi$.

**Теорема полноты (Гёдель):**
Если $\Gamma \models \varphi$, то $\Gamma \vdash \varphi$.

**Следствие (компактность):**
Если каждое конечное подмножество $\Gamma$ выполнимо, то $\Gamma$ выполнимо.

---

## Теория вычислимости

### Рекурсивные функции

**Примитивно рекурсивные функции:**
Базовые функции:
- $Zero(x) = 0$
- $Succ(x) = x + 1$
- $Proj_i^n(x_1, \ldots, x_n) = x_i$

Операции:
- **Композиция**: $h(x) = f(g_1(x), \ldots, g_k(x))$
- **Примитивная рекурсия**: 
  $$h(x, 0) = f(x)$$
  $$h(x, y+1) = g(x, y, h(x, y))$$

**Частично рекурсивные функции:**
Добавляется операция минимизации:
$$h(x) = \mu y [f(x, y) = 0]$$

### Тезис Чёрча-Тьюринга

**Формулировка:**
Класс частично рекурсивных функций совпадает с классом алгоритмически вычислимых функций.

**Эквивалентные модели:**
- Машины Тьюринга
- λ-исчисление
- Частично рекурсивные функции
- Машины с неограниченными регистрами

### Неразрешимые проблемы

**Проблема останова:**
Не существует алгоритма, определяющего для произвольной программы $P$ и входа $x$, завершится ли выполнение $P(x)$.

**Теорема Райса:**
Любое нетривиальное семантическое свойство программ неразрешимо.

---

## Приложения в программировании

### Верификация программ

**Логика Хоара:**
$$\{P\} S \{Q\}$$

где $P$ - предусловие, $S$ - программа, $Q$ - постусловие.

**Правила вывода:**

**Присваивание:**
$$\{Q[x := E]\} x := E \{Q\}$$

**Последовательность:**
$$\frac{\{P\} S_1 \{R\} \quad \{R\} S_2 \{Q\}}{\{P\} S_1; S_2 \{Q\}}$$

**Условие:**
$$\frac{\{P \land B\} S_1 \{Q\} \quad \{P \land \neg B\} S_2 \{Q\}}{\{P\} \text{if } B \text{ then } S_1 \text{ else } S_2 \{Q\}}$$

**Цикл:**
$$\frac{\{I \land B\} S \{I\}}{\{I\} \text{while } B \text{ do } S \{I \land \neg B\}}$$

### Типовые системы

**Система Хиндли-Милнера:**
$$\frac{\Gamma \vdash e_1 : \tau_1 \to \tau_2 \quad \Gamma \vdash e_2 : \tau_1}{\Gamma \vdash e_1 \, e_2 : \tau_2}$$

**Полиморфизм:**
$$\frac{\Gamma \vdash e : \sigma \quad \alpha \notin FV(\Gamma)}{\Gamma \vdash e : \forall \alpha. \sigma}$$

### Спецификация и верификация

```python
def binary_search(arr, target):
    """
    Предусловие: arr отсортирован по возрастанию
    Постусловие: возвращает индекс target в arr или -1
    """
    left, right = 0, len(arr) - 1
    
    # Инвариант: если target в arr, то он в arr[left:right+1]
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# Формальная спецификация
def binary_search_spec(arr, target):
    """
    Предусловие: ∀i,j (0 ≤ i < j < len(arr) → arr[i] ≤ arr[j])
    Постусловие: 
        result = -1 → target ∉ arr
        result ≥ 0 → arr[result] = target
    """
    pass
```

### Автоматические доказательства

**SAT-решатели:**
```python
def dpll(formula):
    """Алгоритм DPLL для SAT"""
    # Единичное распространение
    formula = unit_propagation(formula)
    
    # Устранение чистых литералов
    formula = pure_literal_elimination(formula)
    
    if is_empty(formula):
        return True  # Выполнимо
    
    if has_empty_clause(formula):
        return False  # Невыполнимо
    
    # Выбор переменной для ветвления
    var = choose_variable(formula)
    
    # Попробовать var = True
    if dpll(formula.assign(var, True)):
        return True
    
    # Попробовать var = False
    return dpll(formula.assign(var, False))
```

---

## Связанные темы

- [[discrete-mathematics]] - Дискретная математика
- [[complexity-theory]] - Теория сложности
- [[formal-methods]] - Формальные методы
- [[type-theory]] - Теория типов

---

*Математическая логика обеспечивает формальную основу для рассуждений о программах, алгоритмах и вычислениях. Она необходима для понимания верификации программ, автоматических доказательств и формальных методов разработки.* 