# Теория сложности вычислений

> **Ресурсы, необходимые для решения задач**  
> Классы P, NP, PSPACE и иерархии сложности

[[Теория вычислений]] → **Теория сложности**

---

## 🎯 Ключевые концепции

### ⚡ Основные идеи
- **Временная сложность** - количество шагов алгоритма
- **Пространственная сложность** - объем используемой памяти
- **Классы сложности** - группы задач с похожими ресурсными требованиями
- **Редукции** - сведение одной задачи к другой

### 🔗 Связи с другими темами
- **[[Теория вычислимости]]** - что можно вычислить в принципе
- **[[Анализ алгоритмов]]** - конкретные алгоритмы и их эффективность
- **[[Автоматы]]** - модели вычислений с ограниченными ресурсами

---

## 📊 Основные классы сложности

### Классы P и NP

```python
class ComplexityClasses:
    """Демонстрация основных классов сложности"""
    
    def __init__(self):
        self.classes = {
            'P': {
                'definition': 'Задачи, решаемые за полиномиальное время',
                'machine_model': 'Детерминированная машина Тьюринга',
                'time_bound': 'O(n^k) для некоторой константы k',
                'examples': ['Поиск в массиве', 'Сортировка', 'Кратчайший путь']
            },
            'NP': {
                'definition': 'Задачи, проверяемые за полиномиальное время',
                'machine_model': 'Недетерминированная машина Тьюринга',
                'time_bound': 'O(n^k) для проверки решения',
                'examples': ['SAT', 'Клика', 'Рюкзак', 'TSP']
            },
            'co-NP': {
                'definition': 'Дополнения задач из NP',
                'machine_model': 'НМТ для опровержения',
                'time_bound': 'O(n^k) для проверки "НЕТ"',
                'examples': ['UNSAT', 'Несвязность графа']
            },
            'PSPACE': {
                'definition': 'Задачи, решаемые с полиномиальной памятью',
                'machine_model': 'ДМТ с полиномиальной лентой',
                'space_bound': 'O(n^k) ячеек памяти',
                'examples': ['QBF', 'География', 'Планирование']
            }
        }
    
    def explain_p_vs_np(self):
        """Объяснение различия между P и NP"""
        
        explanation = {
            'intuition': {
                'P': 'Легко найти решение',
                'NP': 'Легко проверить решение (но не обязательно найти)',
                'example': 'Разложение на простые множители vs проверка произведения'
            },
            
            'formal_definitions': {
                'P': 'L ∈ P ⟺ ∃k∃M: M решает L за время O(n^k)',
                'NP': 'L ∈ NP ⟺ ∃k∃V: для x∈L ∃y(|y|≤|x|^k ∧ V(x,y) принимает за O(|x|^k))'
            },
            
            'open_question': 'P =? NP - одна из важнейших открытых проблем в информатике',
            
            'implications': {
                'if_P_equals_NP': [
                    'Криптография станет невозможной',
                    'Многие NP-полные задачи станут эффективно решаемыми',
                    'Революция в оптимизации и планировании'
                ],
                'if_P_not_equals_NP': [
                    'Существуют задачи, трудные для решения, но легкие для проверки',
                    'Односторонние функции существуют',
                    'Криптография имеет математическое обоснование'
                ]
            }
        }
        
        return explanation

# Демонстрация различий между классами
complexity_demo = ComplexityClasses()
p_vs_np = complexity_demo.explain_p_vs_np()

print("=== P vs NP ===")
print(f"Интуиция P: {p_vs_np['intuition']['P']}")
print(f"Интуиция NP: {p_vs_np['intuition']['NP']}")
print(f"Пример: {p_vs_np['intuition']['example']}")
print(f"Открытая проблема: {p_vs_np['open_question']}")
```

### Полные задачи

```python
class NPCompleteProblems:
    """Демонстрация NP-полных задач"""
    
    def __init__(self):
        self.np_complete_problems = {
            'SAT': {
                'description': 'Выполнимость булевых формул',
                'input': 'Булева формула в КНФ',
                'question': 'Существует ли присваивание, делающее формулу истинной?',
                'significance': 'Первая доказанная NP-полная задача (теорема Кука)'
            },
            
            '3-SAT': {
                'description': 'SAT для формул с клозами длины ≤ 3',
                'input': 'Формула в 3-КНФ',
                'question': 'Выполнима ли формула?',
                'significance': 'Упрощенная версия SAT, все еще NP-полная'
            },
            
            'CLIQUE': {
                'description': 'Поиск клики в графе',
                'input': 'Граф G и число k',
                'question': 'Есть ли в G клика размера k?',
                'significance': 'Классическая задача теории графов'
            },
            
            'VERTEX_COVER': {
                'description': 'Вершинное покрытие',
                'input': 'Граф G и число k',
                'question': 'Есть ли покрытие вершин размера ≤ k?',
                'significance': 'Дуальная задаче о максимальном независимом множестве'
            },
            
            'HAMILTONIAN_PATH': {
                'description': 'Гамильтонов путь',
                'input': 'Граф G',
                'question': 'Есть ли путь, проходящий через каждую вершину ровно раз?',
                'significance': 'Обобщение знаменитой задачи коммивояжера'
            },
            
            'TSP': {
                'description': 'Задача коммивояжера (решающая версия)',
                'input': 'Полный граф с весами и число k',
                'question': 'Есть ли гамильтонов цикл длины ≤ k?',
                'significance': 'Одна из самых известных оптимизационных задач'
            }
        }
    
    def demonstrate_sat(self):
        """Демонстрация задачи SAT"""
        
        def evaluate_cnf(formula, assignment):
            """Проверка выполнимости КНФ формулы"""
            for clause in formula:
                clause_satisfied = False
                for literal in clause:
                    if literal > 0:  # Положительный литерал
                        if assignment.get(literal, False):
                            clause_satisfied = True
                            break
                    else:  # Отрицательный литерал
                        if not assignment.get(-literal, False):
                            clause_satisfied = True
                            break
                
                if not clause_satisfied:
                    return False
            return True
        
        def solve_sat_bruteforce(formula, variables):
            """Решение SAT перебором (экспоненциальное время)"""
            n = len(variables)
            
            for i in range(2**n):
                assignment = {}
                for j, var in enumerate(variables):
                    assignment[var] = bool(i & (1 << j))
                
                if evaluate_cnf(formula, assignment):
                    return True, assignment
            
            return False, None
        
        # Пример: (x₁ ∨ ¬x₂ ∨ x₃) ∧ (¬x₁ ∨ x₂) ∧ (¬x₂ ∨ ¬x₃)
        formula = [
            [1, -2, 3],    # x₁ ∨ ¬x₂ ∨ x₃
            [-1, 2],       # ¬x₁ ∨ x₂  
            [-2, -3]       # ¬x₂ ∨ ¬x₃
        ]
        variables = [1, 2, 3]
        
        satisfiable, solution = solve_sat_bruteforce(formula, variables)
        
        print("=== Демонстрация SAT ===")
        print(f"Формула: {formula}")
        print(f"Переменные: {variables}")
        print(f"Выполнима: {satisfiable}")
        if satisfiable:
            readable_solution = {f"x{k}": v for k, v in solution.items()}
            print(f"Решение: {readable_solution}")
        
        return satisfiable, solution
    
    def demonstrate_clique(self):
        """Демонстрация задачи о клике"""
        
        def has_clique(graph, k):
            """Проверка существования клики размера k"""
            from itertools import combinations
            
            vertices = list(graph.keys())
            
            # Перебираем все подмножества размера k
            for subset in combinations(vertices, k):
                is_clique = True
                
                # Проверяем, что все пары вершин соединены
                for i in range(len(subset)):
                    for j in range(i + 1, len(subset)):
                        if subset[j] not in graph[subset[i]]:
                            is_clique = False
                            break
                    if not is_clique:
                        break
                
                if is_clique:
                    return True, subset
            
            return False, None
        
        # Пример графа
        graph = {
            'A': ['B', 'C', 'D'],
            'B': ['A', 'C'],
            'C': ['A', 'B', 'D'],
            'D': ['A', 'C'],
            'E': ['F'],
            'F': ['E']
        }
        
        print("\n=== Демонстрация CLIQUE ===")
        print(f"Граф: {graph}")
        
        for k in range(2, 5):
            has_k_clique, clique = has_clique(graph, k)
            print(f"Клика размера {k}: {has_k_clique}")
            if has_k_clique:
                print(f"  Найденная клика: {clique}")
        
        return graph

# Демонстрация NP-полных задач
np_problems = NPCompleteProblems()
np_problems.demonstrate_sat()
np_problems.demonstrate_clique()
```

---

## 🔄 Сведения между задачами

### Полиномиальные сведения

```python
class PolynomialReductions:
    """Демонстрация полиномиальных сведений между задачами"""
    
    def sat_to_3sat(self, formula):
        """Сведение SAT к 3-SAT"""
        
        result_formula = []
        new_var_counter = max(abs(lit) for clause in formula for lit in clause) + 1
        
        for clause in formula:
            if len(clause) <= 3:
                # Клоз уже подходящей длины
                result_formula.append(clause)
            
            elif len(clause) == 4:
                # (a ∨ b ∨ c ∨ d) → (a ∨ b ∨ y) ∧ (¬y ∨ c ∨ d)
                y = new_var_counter
                new_var_counter += 1
                
                result_formula.append([clause[0], clause[1], y])
                result_formula.append([-y, clause[2], clause[3]])
            
            else:
                # Для более длинных клозов: цепочка вспомогательных переменных
                # (a₁ ∨ a₂ ∨ ... ∨ aₙ) → 
                # (a₁ ∨ a₂ ∨ y₁) ∧ (¬y₁ ∨ a₃ ∨ y₂) ∧ ... ∧ (¬yₙ₋₃ ∨ aₙ₋₁ ∨ aₙ)
                
                prev_var = clause[0]
                for i in range(1, len(clause) - 2):
                    y = new_var_counter
                    new_var_counter += 1
                    
                    if i == 1:
                        result_formula.append([prev_var, clause[i], y])
                    else:
                        result_formula.append([-prev_var, clause[i], y])
                    
                    prev_var = y
                
                # Последний клоз
                result_formula.append([-prev_var, clause[-2], clause[-1]])
        
        return result_formula
    
    def sat_to_clique(self, formula):
        """Сведение 3-SAT к задаче о клике"""
        
        # Создаем граф для задачи о клике
        vertices = []
        edges = []
        
        # Для каждого клоза создаем вершины (по одной на каждый литерал)
        for i, clause in enumerate(formula):
            for j, literal in enumerate(clause):
                vertex_name = f"c{i}_l{j}_{literal}"
                vertices.append(vertex_name)
        
        # Создаем ребра между вершинами из разных клозов,
        # если они не противоречат друг другу
        for i in range(len(vertices)):
            for j in range(i + 1, len(vertices)):
                v1_parts = vertices[i].split('_')
                v2_parts = vertices[j].split('_')
                
                clause1 = int(v1_parts[0][1:])  # Номер клоза
                clause2 = int(v2_parts[0][1:])
                literal1 = int(v1_parts[2])     # Литерал
                literal2 = int(v2_parts[2])
                
                # Ребро есть, если:
                # 1. Вершины из разных клозов
                # 2. Литералы не противоречат (не x и ¬x)
                if clause1 != clause2 and literal1 != -literal2:
                    edges.append((vertices[i], vertices[j]))
        
        clique_size = len(formula)  # Размер клики = количество клозов
        
        return {
            'vertices': vertices,
            'edges': edges,
            'clique_size': clique_size,
            'explanation': 'Клика размера k существует ⟺ исходная формула выполнима'
        }
    
    def clique_to_vertex_cover(self, graph, clique_size):
        """Сведение CLIQUE к VERTEX-COVER"""
        
        # Строим дополнение графа
        all_vertices = set(graph.keys())
        complement_edges = []
        
        for v1 in all_vertices:
            for v2 in all_vertices:
                if v1 != v2 and v2 not in graph.get(v1, []):
                    complement_edges.append((v1, v2))
        
        # В дополнении графа ищем вершинное покрытие размера n - k
        vertex_cover_size = len(all_vertices) - clique_size
        
        return {
            'complement_graph': complement_edges,
            'vertex_cover_size': vertex_cover_size,
            'explanation': 'G имеет клику размера k ⟺ Ḡ имеет покрытие размера n-k'
        }

# Демонстрация сведений
reductions = PolynomialReductions()

# Пример сведения SAT → 3-SAT
original_formula = [[1, 2, 3, 4, 5]]  # Длинный клоз
threeSAT_formula = reductions.sat_to_3sat(original_formula)

print("=== Сведение SAT → 3-SAT ===")
print(f"Исходная формула: {original_formula}")
print(f"3-SAT формула: {threeSAT_formula}")

# Пример сведения 3-SAT → CLIQUE
formula_3sat = [[1, -2, 3], [-1, 2], [2, -3]]
clique_instance = reductions.sat_to_clique(formula_3sat)

print(f"\n=== Сведение 3-SAT → CLIQUE ===")
print(f"Формула 3-SAT: {formula_3sat}")
print(f"Граф вершины: {len(clique_instance['vertices'])}")
print(f"Граф ребра: {len(clique_instance['edges'])}")
print(f"Искомый размер клики: {clique_instance['clique_size']}")
```

---

## 🏗️ Иерархии сложности

### Временная иерархия

```python
class TimeHierarchy:
    """Демонстрация временной иерархии теоремы"""
    
    def __init__(self):
        self.hierarchy_levels = {
            'DTIME(n)': 'Линейное время',
            'DTIME(n log n)': 'Квазилинейное время (сортировка)',
            'DTIME(n²)': 'Квадратичное время',
            'DTIME(n^k)': 'Полиномиальное время (класс P)',
            'DTIME(2^n)': 'Экспоненциальное время',
            'DTIME(2^2^n)': 'Двойное экспоненциальное время'
        }
    
    def time_hierarchy_theorem(self):
        """Формулировка теоремы о временной иерархии"""
        
        theorem = {
            'statement': 'Если f(n) и g(n) - конструируемые по времени функции и f(n) log f(n) = o(g(n)), то DTIME(f(n)) ⊊ DTIME(g(n))',
            
            'intuition': 'Больше времени позволяет решать строго больше задач',
            
            'proof_idea': [
                '1. Строим диагональную машину D',
                '2. D(x) симулирует M_x на x не более g(|x|) шагов',
                '3. Если M_x(x) останавливается за ≤ f(|x|) шагов, то D делает наоборот',
                '4. D ∈ DTIME(g(n)) но D ∉ DTIME(f(n))'
            ],
            
            'examples': {
                'DTIME(n) ⊊ DTIME(n²)': 'Квадратичное время строго мощнее линейного',
                'P ⊊ EXPTIME': 'Экспоненциальное время строго мощнее полиномиального',
                'EXPTIME ⊊ DOUBEXP': 'Двойная экспонента строго мощнее одинарной'
            }
        }
        
        return theorem
    
    def space_hierarchy_theorem(self):
        """Теорема о пространственной иерархии"""
        
        theorem = {
            'statement': 'Если f(n) и g(n) - конструируемые по пространству и f(n) = o(g(n)), то DSPACE(f(n)) ⊊ DSPACE(g(n))',
            
            'differences_from_time': [
                'Не нужен log-фактор (пространство можно переиспользовать)',
                'Более тонкая иерархия',
                'PSPACE имеет полные задачи'
            ],
            
            'examples': {
                'DSPACE(n) ⊊ DSPACE(n²)': 'Квадратичное пространство строго больше линейного',
                'L ⊊ PSPACE': 'Полиномиальное пространство строго больше логарифмического',
                'PSPACE ⊊ EXPSPACE': 'Экспоненциальное пространство строго больше полиномиального'
            }
        }
        
        return theorem

# Демонстрация иерархий
hierarchy = TimeHierarchy()
time_theorem = hierarchy.time_hierarchy_theorem()
space_theorem = hierarchy.space_hierarchy_theorem()

print("=== Теорема о временной иерархии ===")
print(f"Утверждение: {time_theorem['statement']}")
print(f"Интуиция: {time_theorem['intuition']}")
print("Примеры:")
for example, description in time_theorem['examples'].items():
    print(f"  {example}: {description}")
```

### Пространственная сложность

```python
class SpaceComplexity:
    """Анализ пространственной сложности"""
    
    def __init__(self):
        self.space_classes = {
            'L': 'Логарифмическое пространство DSPACE(log n)',
            'NL': 'Недетерминированное логарифмическое пространство',
            'PSPACE': 'Полиномиальное пространство DSPACE(n^k)',
            'EXPSPACE': 'Экспоненциальное пространство DSPACE(2^n^k)'
        }
    
    def savitch_theorem(self):
        """Теорема Савича о связи детерминированного и недетерминированного пространства"""
        
        theorem = {
            'statement': 'NSPACE(s(n)) ⊆ DSPACE((s(n))²) для s(n) ≥ log n',
            
            'algorithm': [
                'Задача: достижима ли вершина t из s за ≤ 2^i шагов?',
                'REACH(s, t, i):',
                '  if i = 0: return s = t or (s,t) ∈ E',
                '  for each vertex v:',
                '    if REACH(s, v, i-1) and REACH(v, t, i-1):',
                '      return true',
                '  return false'
            ],
            
            'space_analysis': [
                'Глубина рекурсии: O(log n)',
                'На каждом уровне: O(s(n)) для хранения вершин',
                'Общее пространство: O(s(n) × log n) = O((s(n))²)'
            ],
            
            'consequences': [
                'PSPACE = NPSPACE',
                'NL ⊆ SPACE(log² n)',
                'Квадратичный разрыв между детерминированным и недетерминированным пространством'
            ]
        }
        
        return theorem
    
    def pspace_complete_problems(self):
        """PSPACE-полные задачи"""
        
        problems = {
            'QBF': {
                'name': 'Quantified Boolean Formula',
                'description': 'Истинность полностью квантифицированной булевой формулы',
                'example': '∃x₁∀x₂∃x₃((x₁ ∨ x₂) ∧ (¬x₂ ∨ x₃))',
                'significance': 'Каноническая PSPACE-полная задача'
            },
            
            'GEOGRAPHY': {
                'name': 'Geography Game',
                'description': 'Есть ли выигрышная стратегия в игре "География"',
                'example': 'Граф городов, игроки ходят по ребрам, проигрывает тот, кто не может ходить',
                'significance': 'Пример игровой задачи в PSPACE'
            },
            
            'TQBF': {
                'name': 'True Quantified Boolean Formula',
                'description': 'Истинность квантифицированной формулы',
                'example': '∀x∃y(x → y)',
                'significance': 'Обобщение SAT с кванторами'
            }
        }
        
        return problems

# Демонстрация пространственной сложности
space_complexity = SpaceComplexity()
savitch = space_complexity.savitch_theorem()
pspace_problems = space_complexity.pspace_complete_problems()

print("\n=== Теорема Савича ===")
print(f"Утверждение: {savitch['statement']}")
print("Алгоритм:")
for step in savitch['algorithm']:
    print(f"  {step}")
print("Следствия:")
for consequence in savitch['consequences']:
    print(f"  • {consequence}")
```

---

## 🎯 Практические применения

### Криптография и односторонние функции

```python
class CryptographicComplexity:
    """Связь теории сложности с криптографией"""
    
    def one_way_functions(self):
        """Односторонние функции и их связь с P vs NP"""
        
        concepts = {
            'definition': 'f одностороння если f вычислима за полиномиальное время, но f⁻¹ требует суперполиномиального времени',
            
            'examples': [
                'Произведение больших простых чисел (факторизация)',
                'Дискретный логарифм в конечных полях',
                'Хеш-функции (предположительно)',
                'Задача рюкзака с плотностью близкой к 1'
            ],
            
            'connection_to_P_vs_NP': [
                'Если P = NP, то односторонние функции не существуют',
                'Если P ≠ NP, односторонние функции могут существовать',
                'Существование односторонних функций ⟹ P ≠ NP',
                'P ≠ NP ⟹ существование односторонних функций (неизвестно)'
            ],
            
            'cryptographic_applications': [
                'Генерация псевдослучайных чисел',
                'Цифровые подписи',
                'Схемы обязательств (commitment schemes)',
                'Протоколы с нулевым разглашением'
            ]
        }
        
        return concepts
    
    def rsa_complexity(self):
        """Анализ сложности RSA"""
        
        def rsa_demo():
            """Упрощенная демонстрация RSA"""
            
            # Маленькие простые числа для демонстрации
            p, q = 7, 11
            n = p * q  # 77
            phi_n = (p - 1) * (q - 1)  # 60
            
            # Выбираем e взаимно простое с φ(n)
            e = 7  # gcd(7, 60) = 1
            
            # Находим d: ed ≡ 1 (mod φ(n))
            # 7d ≡ 1 (mod 60)
            d = 43  # 7 * 43 = 301 ≡ 1 (mod 60)
            
            # Шифрование: c = m^e mod n
            message = 5
            ciphertext = pow(message, e, n)  # 5^7 mod 77 = 47
            
            # Расшифрование: m = c^d mod n
            decrypted = pow(ciphertext, d, n)  # 47^43 mod 77 = 5
            
            return {
                'public_key': (n, e),
                'private_key': (n, d),
                'message': message,
                'ciphertext': ciphertext,
                'decrypted': decrypted,
                'security_assumption': 'Факторизация больших чисел трудна'
            }
        
        demo = rsa_demo()
        
        complexity_analysis = {
            'key_generation': 'O(k³) для k-битных ключей (тест простоты)',
            'encryption': 'O(k³) (быстрое возведение в степень)',
            'decryption': 'O(k³) (быстрое возведение в степень)',
            'factorization': 'Субэкспоненциальное время (лучшие алгоритмы)',
            'quantum_threat': 'Алгоритм Шора решает за полиномиальное время на квантовом компьютере'
        }
        
        return demo, complexity_analysis

# Демонстрация криптографических применений
crypto = CryptographicComplexity()
owf = crypto.one_way_functions()
rsa_demo, rsa_complexity = crypto.rsa_complexity()

print("=== Односторонние функции ===")
print(f"Определение: {owf['definition']}")
print("Примеры:")
for example in owf['examples']:
    print(f"  • {example}")

print(f"\n=== RSA демонстрация ===")
print(f"Публичный ключ: {rsa_demo['public_key']}")
print(f"Сообщение: {rsa_demo['message']}")
print(f"Шифртекст: {rsa_demo['ciphertext']}")
print(f"Расшифровано: {rsa_demo['decrypted']}")
```

---

## 🔗 Связанные темы

### 📚 Теоретические основы
- **[[Теория вычислимости]]** - разрешимые vs неразрешимые задачи
- **[[Автоматы]]** - простейшие модели с ограниченными ресурсами
- **[[Формальные языки и грамматики]]** - иерархия языков и их сложность

### 🛠️ Практические применения
- **[[Анализ алгоритмов]]** - конкретные алгоритмы и их сложность
- **[[Криптография]]** - односторонние функции и трудные задачи
- **[[Оптимизация]]** - приближенные алгоритмы для NP-трудных задач

### 🧮 Смежные области
- **Квантовые вычисления** - классы BQP, QMA
- **Параллельные вычисления** - классы NC, AC
- **Дескриптивная сложность** - связь с логикой

---

## 🎓 Практические задания

### 📝 Начальный уровень
1. **Основы классов сложности**
   - [ ] Доказать, что MERGE-SORT ∈ P
   - [ ] Показать, что SUBSET-SUM ∈ NP
   - [ ] Объяснить различие между P и NP

2. **NP-полные задачи**
   - [ ] Решить малый пример SAT
   - [ ] Найти клику в графе перебором
   - [ ] Понять сведение 3-SAT → CLIQUE

### 🔧 Средний уровень
3. **Сведения**
   - [ ] Доказать NP-полноту VERTEX-COVER
   - [ ] Построить сведение SAT → HAMILTONIAN-PATH
   - [ ] Исследовать сведения в PSPACE

4. **Иерархии сложности**
   - [ ] Понять теорему о временной иерархии
   - [ ] Изучить теорему Савича
   - [ ] Анализ PSPACE-полных задач

### 🚀 Продвинутый уровень
5. **Исследования**
   - [ ] Изучение связи с криптографией
   - [ ] Приближенные алгоритмы
   - [ ] Квантовая сложность

---

*"Теория сложности показывает, что не все задачи одинаково трудны - некоторые фундаментально требуют больше ресурсов, чем другие"* ⚡🧮 