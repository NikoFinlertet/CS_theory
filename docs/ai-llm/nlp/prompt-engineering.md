# 🎯 Prompt Engineering: Искусство составления запросов

## 📝 Введение в Prompt Engineering

### Что такое Prompt Engineering?
- **Определение** - процесс разработки и оптимизации текстовых запросов для максимально эффективного взаимодействия с языковыми моделями
- **Цель** - получение точных, релевантных и полезных ответов от AI систем
- **Важность** - разница между хорошим и плохим промптом может кардинально изменить качество результата
- **Навык будущего** - критически важная компетенция для работы с AI

### Основные принципы
```
Ключевые принципы эффективных промптов:
├── Ясность и специфичность
├── Контекст и детали
├── Структурированность
├── Примеры (few-shot learning)
├── Итеративная оптимизация
└── Понимание ограничений модели
```

---

## 🏗️ Анатомия промпта

### Структура эффективного промпта
```
Компоненты промпта:
├── Системный промпт (роль/контекст)
├── Инструкция (что делать)
├── Контекст (дополнительная информация)
├── Примеры (демонстрация желаемого результата)
├── Входные данные (конкретная задача)
└── Ограничения (формат ответа, длина)
```

### Базовый шаблон
```
Роль: Ты эксперт по [область знаний]
Задача: [четкая инструкция]
Контекст: [дополнительная информация]
Примеры: [1-3 примера желаемого результата]
Формат: [как должен выглядеть ответ]
Ограничения: [что важно учесть]

Входные данные: [конкретные данные для обработки]
```

---

## 🎨 Техники промптинга

### Zero-shot промптинг
```
Описание: Решение задачи без предоставления примеров

Пример:
"Переведи следующий текст с английского на русский: 
'The weather is beautiful today.'"

Результат: "Сегодня прекрасная погода."
```

### Few-shot промптинг
```
Описание: Предоставление нескольких примеров для демонстрации паттерна

Пример:
"Классифицируй эмоциональную окраску отзывов:

Отзыв: 'Отличный продукт, очень доволен!'
Эмоция: Положительная

Отзыв: 'Ужасное качество, не рекомендую'
Эмоция: Отрицательная

Отзыв: 'Товар в порядке, ничего особенного'
Эмоция: Нейтральная

Отзыв: 'Превзошел все ожидания!'
Эмоция:"

Результат: "Положительная"
```

### Chain-of-Thought (CoT)
```
Описание: Пошаговое рассуждение для сложных задач

Пример:
"Реши задачу пошагово:
В магазине было 23 яблока. Продали 8 яблок утром и 7 яблок вечером. 
Сколько яблок осталось?

Решение:
1. Изначально было: 23 яблока
2. Продали утром: 8 яблок
3. Продали вечером: 7 яблок
4. Всего продали: 8 + 7 = 15 яблок
5. Осталось: 23 - 15 = 8 яблок

Ответ: 8 яблок"
```

### Self-Consistency
```
Описание: Генерация нескольких решений и выбор наиболее частого

Промпт:
"Реши эту задачу тремя разными способами и выбери наиболее вероятный ответ:
[задача]"
```

---

## 🔧 Продвинутые техники

### Role-Based промптинг
```python
# Пример системного промпта для разных ролей

EXPERT_ROLES = {
    "data_scientist": """
    Ты опытный специалист по данным с 10+ летним опытом.
    Специализируешься на машинном обучении, статистике и анализе данных.
    Всегда предоставляешь практичные, основанные на данных рекомендации.
    """,
    
    "software_architect": """
    Ты senior software architect с экспертизой в проектировании
    масштабируемых систем. Фокусируешься на best practices,
    производительности и поддерживаемости кода.
    """,
    
    "business_analyst": """
    Ты бизнес-аналитик с пониманием технических и бизнес-процессов.
    Умеешь переводить техническую сложность в бизнес-ценность.
    """
}

def create_expert_prompt(role, question):
    return f"{EXPERT_ROLES[role]}\n\nВопрос: {question}"
```

### Tree of Thoughts
```
Описание: Исследование нескольких путей решения

Структура:
1. Определи проблему
2. Сгенерируй несколько подходов
3. Оцени каждый подход
4. Выбери лучший путь
5. Детализируй решение

Пример промпта:
"Используя метод 'дерева мыслей', найди решение для [проблема]:
1. Опиши 3 различных подхода
2. Оцени плюсы и минусы каждого
3. Выбери оптимальный подход
4. Детализируй пошаговое решение"
```

### Constitutional AI
```
Описание: Направление модели на этичные и безопасные ответы

Пример:
"Следуй этим принципам при ответе:
1. Будь честным и точным
2. Избегай вредного контента
3. Уважай различные точки зрения
4. Признавай ограничения своих знаний

Вопрос: [вопрос]"
```

---

## 💡 Практические стратегии

### Итеративная оптимизация
```python
def optimize_prompt(base_prompt, test_cases, model):
    """
    Итеративное улучшение промпта
    """
    best_prompt = base_prompt
    best_score = evaluate_prompt(base_prompt, test_cases, model)
    
    # Стратегии улучшения
    improvements = [
        "Добавить больше контекста",
        "Включить примеры",
        "Уточнить инструкции",
        "Изменить формат вывода",
        "Добавить ограничения"
    ]
    
    for improvement in improvements:
        new_prompt = apply_improvement(best_prompt, improvement)
        score = evaluate_prompt(new_prompt, test_cases, model)
        
        if score > best_score:
            best_prompt = new_prompt
            best_score = score
    
    return best_prompt

def evaluate_prompt(prompt, test_cases, model):
    """
    Оценка качества промпта на тестовых случаях
    """
    scores = []
    for test_case in test_cases:
        response = model.generate(prompt + test_case['input'])
        score = calculate_quality_score(response, test_case['expected'])
        scores.append(score)
    
    return sum(scores) / len(scores)
```

### A/B тестирование промптов
```python
def ab_test_prompts(prompt_a, prompt_b, test_data, model):
    """
    Сравнение двух промптов
    """
    results_a = []
    results_b = []
    
    for data in test_data:
        # Тест промпта A
        response_a = model.generate(prompt_a + data['input'])
        score_a = evaluate_response(response_a, data['expected'])
        results_a.append(score_a)
        
        # Тест промпта B
        response_b = model.generate(prompt_b + data['input'])
        score_b = evaluate_response(response_b, data['expected'])
        results_b.append(score_b)
    
    # Статистическое сравнение
    return {
        'prompt_a_mean': np.mean(results_a),
        'prompt_b_mean': np.mean(results_b),
        'winner': 'A' if np.mean(results_a) > np.mean(results_b) else 'B',
        'confidence': calculate_confidence(results_a, results_b)
    }
```

---

## 🎯 Специализированные промпты

### Для кодирования
```
Роль: Ты senior Python разработчик
Задача: Написать чистый, хорошо документированный код

Требования:
- Следуй PEP 8
- Добавь типизацию
- Включи docstrings
- Обработай ошибки
- Напиши тесты

Пример:
```python
def calculate_average(numbers: List[float]) -> float:
    """
    Вычисляет среднее арифметическое списка чисел.
    
    Args:
        numbers: Список чисел для вычисления среднего
        
    Returns:
        Среднее арифметическое
        
    Raises:
        ValueError: Если список пуст
    """
    if not numbers:
        raise ValueError("Список не может быть пустым")
    
    return sum(numbers) / len(numbers)
```

Теперь напиши функцию для [описание задачи]:
```

### Для анализа данных
```
Роль: Ты data scientist с экспертизой в статистическом анализе

Задача: Проанализируй данные и предоставь инсайты

Структура ответа:
1. Краткое резюме данных
2. Ключевые метрики и статистики
3. Выявленные паттерны и аномалии
4. Визуализации (описание графиков)
5. Выводы и рекомендации
6. Ограничения анализа

Данные: [описание или образец данных]
```

### Для творческих задач
```
Роль: Ты креативный копирайтер и сторителлер

Контекст: Создание контента для [целевая аудитория] в стиле [желаемый стиль]

Требования:
- Оригинальность и креативность
- Соответствие бренду/тону
- Эмоциональная привлекательность
- Четкий call-to-action

Задача: [конкретное задание]
```

---

## 📊 Метрики и оценка

### Качественные метрики
```
Критерии оценки промптов:
├── Релевантность ответа
├── Полнота информации
├── Структурированность
├── Творческий подход
├── Соответствие формату
└── Практическая применимость
```

### Количественные метрики
```python
def calculate_prompt_metrics(responses, expected_outputs):
    """
    Расчет метрик качества промпта
    """
    metrics = {
        'accuracy': calculate_accuracy(responses, expected_outputs),
        'bleu_score': calculate_bleu(responses, expected_outputs),
        'rouge_score': calculate_rouge(responses, expected_outputs),
        'semantic_similarity': calculate_semantic_similarity(responses, expected_outputs),
        'response_length': np.mean([len(r.split()) for r in responses]),
        'response_time': measure_response_time(responses)
    }
    
    return metrics

def track_prompt_performance(prompt_id, metrics):
    """
    Отслеживание производительности промптов во времени
    """
    # Сохранение в базу данных или логи
    performance_log = {
        'prompt_id': prompt_id,
        'timestamp': datetime.now(),
        'metrics': metrics,
        'version': get_prompt_version(prompt_id)
    }
    
    save_to_database(performance_log)
```

---

## 🛠️ Инструменты и платформы

### Инструменты разработки
```python
# Популярные библиотеки для промптинга

# LangChain - фреймворк для LLM приложений
from langchain.prompts import PromptTemplate, ChatPromptTemplate
from langchain.schema import SystemMessage, HumanMessage

# Создание шаблона промпта
template = PromptTemplate(
    input_variables=["task", "context"],
    template="""
    Ты эксперт по {task}.
    
    Контекст: {context}
    
    Предоставь детальный анализ и рекомендации.
    """
)

# DSPy - фреймворк для оптимизации промптов
import dspy

class CoT(dspy.Module):
    def __init__(self):
        super().__init__()
        self.generate_answer = dspy.ChainOfThought("question -> answer")
    
    def forward(self, question):
        return self.generate_answer(question=question)

# Guidance - структурированная генерация
import guidance

guidance.llm = guidance.llms.OpenAI("gpt-3.5-turbo")

with guidance.user():
    question = guidance("What is {{topic}}?", topic="machine learning")

with guidance.assistant():
    answer = guidance(gen='answer', max_tokens=200)
```

### Платформы для экспериментов
```
Рекомендуемые платформы:
├── OpenAI Playground - тестирование GPT моделей
├── Anthropic Console - эксперименты с Claude
├── Hugging Face Spaces - тестирование open-source моделей
├── PromptBase - маркетплейс промптов
├── LangSmith - отслеживание и оптимизация
└── Weights & Biases - логирование экспериментов
```

---

## 🚀 Практические применения

### Автоматизация бизнес-процессов
```python
# Пример системы автоматической обработки заявок
def process_customer_request(request_text, customer_history):
    prompt = f"""
    Ты система автоматической обработки заявок клиентов.
    
    История клиента: {customer_history}
    Текущая заявка: {request_text}
    
    Задачи:
    1. Определи тип заявки (жалоба/вопрос/предложение)
    2. Оцени приоритет (низкий/средний/высокий)
    3. Предложи действия для решения
    4. Сформулируй ответ клиенту
    
    Формат ответа:
    Тип: [тип]
    Приоритет: [приоритет]
    Действия: [список действий]
    Ответ: [текст ответа клиенту]
    """
    
    return llm.generate(prompt)
```

### Персонализированный контент
```python
def generate_personalized_email(user_profile, campaign_type):
    prompt = f"""
    Создай персонализированное письмо для маркетинговой кампании.
    
    Профиль пользователя:
    - Возраст: {user_profile['age']}
    - Интересы: {user_profile['interests']}
    - История покупок: {user_profile['purchase_history']}
    - Предпочитаемый стиль: {user_profile['communication_style']}
    
    Тип кампании: {campaign_type}
    
    Требования:
    - Персональное обращение
    - Релевантные предложения
    - Эмоциональная привлекательность
    - Четкий call-to-action
    
    Длина: до 150 слов
    """
    
    return llm.generate(prompt)
```

---

## 🔮 Будущее Prompt Engineering

### Emerging тренды
```
Новые направления:
├── Мультимодальные промпты (текст + изображения)
├── Программируемые промпты
├── Автоматическая оптимизация промптов
├── Промпты для специфических доменов
├── Интерактивное промптирование
└── Этичные и безопасные промпты
```

### Автоматизированная оптимизация
```python
# Пример автоматической оптимизации промптов
class PromptOptimizer:
    def __init__(self, model, evaluation_dataset):
        self.model = model
        self.eval_data = evaluation_dataset
    
    def genetic_optimization(self, base_prompt, generations=50):
        population = self.generate_prompt_variants(base_prompt)
        
        for generation in range(generations):
            # Оценка популяции
            scores = [self.evaluate_prompt(p) for p in population]
            
            # Селекция лучших
            best_prompts = self.select_best(population, scores)
            
            # Мутация и кроссовер
            population = self.evolve_population(best_prompts)
        
        return max(population, key=self.evaluate_prompt)
    
    def reinforcement_learning_optimization(self, base_prompt):
        # RL агент для оптимизации промптов
        agent = PromptRLAgent()
        
        for episode in range(1000):
            action = agent.select_modification(base_prompt)
            new_prompt = self.apply_modification(base_prompt, action)
            reward = self.evaluate_prompt(new_prompt)
            agent.update(action, reward)
        
        return agent.get_best_prompt()
```

---

## 📚 Лучшие практики

### Do's (Рекомендации)
```
✅ Лучшие практики:
├── Будь конкретным в инструкциях
├── Предоставляй достаточный контекст
├── Используй примеры для сложных задач
├── Итеративно улучшай промпты
├── Тестируй на различных данных
├── Документируй эффективные паттерны
├── Учитывай ограничения модели
└── Отслеживай производительность
```

### Don'ts (Чего избегать)
```
❌ Чего следует избегать:
├── Слишком расплывчатые инструкции
├── Противоречивые требования
├── Слишком длинные промпты
├── Предвзятые или дискриминационные запросы
├── Запросы на создание вредного контента
├── Игнорирование контекста
├── Отсутствие валидации результатов
└── Копирование промптов без адаптации
```

---

## 🎓 Ресурсы для изучения

### Практические курсы
- **OpenAI Prompt Engineering Guide** - официальное руководство
- **DeepLearning.AI ChatGPT Course** - курс по промптингу
- **Anthropic Claude Documentation** - работа с Claude
- **LangChain Documentation** - разработка LLM приложений

### Связанные темы
- [[large-language-models|Большие языковые модели]] - понимание возможностей LLM
- [[fine-tuning-rag|Fine-tuning и RAG]] - альтернативы промптингу
- [[ai-in-production|AI в продакшене]] - внедрение промптов в системы
- [[ai-ethics-safety|Этика и безопасность]] - ответственное использование

---

💡 **Совет:** Prompt Engineering - это искусство и наука одновременно. Начните с простых техник, экспериментируйте с различными подходами, и всегда тестируйте промпты на реальных данных перед внедрением в продакшен. 