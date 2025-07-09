# Английский язык: Корпусная лингвистика и анализ текста

## 📊 Основы корпусной лингвистики

### Определение и принципы
```
Корпусная лингвистика:
├── Эмпирическое изучение языка
├── Использование больших массивов текстов
├── Количественные методы анализа
├── Статистические закономерности
├── Дистрибуционный анализ
├── Контекстуальное исследование
└── Индуктивный подход

Основные принципы:
├── Representativeness: репрезентативность
├── Balance: сбалансированность
├── Sampling: выборка
├── Authenticity: аутентичность
├── Machine-readable: компьютерная читаемость
├── Finitude: конечность
└── Annotation: разметка
```

### История развития
```
Этапы развития:
├── Пред-компьютерная эра (1900-1960):
│   ├── Ручная обработка текстов
│   ├── Частотные словари
│   ├── Статистические исследования
│   ├── Работы Zipf, Yule
│   └── Основы количественной лингвистики
├── Ранняя компьютерная эра (1960-1980):
│   ├── Первые машиночитаемые корпуса
│   ├── Brown Corpus (1961)
│   ├── KWIC concordances
│   ├── Автоматическая обработка
│   └── Вычислительные методы
├── Современная эра (1980-настоящее):
│   ├── Большие корпуса (миллионы слов)
│   ├── Интернет-корпуса
│   ├── Специализированные корпуса
│   ├── Мультимодальные данные
│   └── Глубокое обучение
```

### Типы корпусов
```
По размеру:
├── Малые корпуса: < 1 млн слов
├── Средние корпуса: 1-100 млн слов
├── Большие корпуса: 100 млн - 1 млрд слов
├── Гигантские корпуса: > 1 млрд слов
├── Веб-корпуса: триллионы слов
└── Потоковые корпуса: непрерывное обновление

По содержанию:
├── Общие корпуса: представляют язык в целом
├── Специализированные корпуса: конкретные домены
├── Мониторные корпуса: регулярно обновляются
├── Исторические корпуса: диахронические данные
├── Учебные корпуса: язык изучающих
├── Устные корпуса: разговорная речь
└── Письменные корпуса: письменные тексты

По структуре:
├── Одноязычные корпуса: один язык
├── Многоязычные корпуса: несколько языков
├── Сопоставимые корпуса: похожие тексты
├── Параллельные корпуса: переводы
├── Синхронные корпуса: одно время
├── Диахронические корпуса: разные периоды
└── Региональные корпуса: географические варианты
```

## 🗂️ Основные английские корпуса

### Исторические корпуса
```
Brown Corpus (1961):
├── Размер: 1 миллион слов
├── Жанры: 15 категорий
├── Временной период: 1961 год
├── Аннотация: POS-теги
├── Значение: первый электронный корпус
├── Влияние: эталон для других корпусов
└── Доступность: свободно доступен

LOB Corpus (1970s):
├── Размер: 1 миллион слов
├── Британский английский
├── Структура: по модели Brown
├── Временной период: 1961 год
├── Сопоставимость: с Brown Corpus
├── Аннотация: POS-теги
└── Значение: первый британский корпус

London-Lund Corpus (1970s):
├── Размер: 500,000 слов
├── Тип: устная речь
├── Британский английский
├── Аннотация: просодическая разметка
├── Значение: первый корпус устной речи
├── Транскрипция: детальная
└── Влияние: модель для других
```

### Современные корпуса
```
British National Corpus (BNC):
├── Размер: 100 миллионов слов
├── Период: 1980s-1990s
├── Соотношение: 90% письменная, 10% устная
├── Жанры: широкий спектр
├── Аннотация: POS-теги, лемматизация
├── Доступность: академическая лицензия
├── Значение: эталон британского английского
└── Версии: XML, различные форматы

Corpus of Contemporary American English (COCA):
├── Размер: 1 миллиард слов
├── Период: 1990-2019
├── Обновления: регулярные
├── Источники: книги, журналы, газеты, ТВ, веб
├── Сбалансированность: 20% на жанр
├── Интерфейс: веб-интерфейс
├── Создатель: Mark Davies
└── Значение: крупнейший корпус американского английского

Google Books Ngram Corpus:
├── Размер: 500 миллиардов слов
├── Период: 1500-2019
├── Источники: оцифрованные книги
├── Языки: множественные
├── Доступность: веб-интерфейс
├── Ограничения: только n-граммы
├── Значение: исторические изменения
└── Проблемы: OCR-ошибки, смещение выборки
```

### Специализированные корпуса
```
CHILDES (Child Language Data Exchange System):
├── Размер: миллионы высказываний
├── Фокус: детская речь
├── Языки: множественные
├── Аннотация: морфологическая, синтаксическая
├── Формат: CHAT
├── Доступность: свободно доступен
├── Создатель: Brian MacWhinney
└── Значение: изучение языкового развития

International Corpus of English (ICE):
├── Размер: 1 миллион слов на вариант
├── Варианты: множественные (ICE-GB, ICE-US, etc.)
├── Структура: стандартизированная
├── Соотношение: 60% устная, 40% письменная
├── Аннотация: POS-теги, парсинг
├── Цель: сравнение вариантов английского
├── Период: 1990s
└── Координация: Survey of English Usage

Academic Word List (AWL):
├── Размер: 3.5 миллионов слов
├── Источники: академические тексты
├── Дисциплины: различные области
├── Цель: выделение академической лексики
├── Результат: 570 семейств слов
├── Создатель: Averil Coxhead
├── Значение: преподавание EAP
└── Критерии: частота и диапазон
```

## 🔍 Методы корпусного анализа

### Частотный анализ
```
Основные показатели:
├── Абсолютная частота: raw frequency
├── Относительная частота: per million words
├── Ранговая частота: rank ordering
├── Нормализованная частота: normalized
├── Логарифмическая частота: log frequency
├── Логарифм шансов: log odds
└── Статистическая значимость: p-values

Распределения:
├── Закон Ципфа: rank × frequency = constant
├── Логнормальное распределение
├── Степенное распределение
├── Распределение Пуассона
├── Биномиальное распределение
├── Гипергеометрическое распределение
└── Отрицательное биномиальное

Визуализация:
├── Гистограммы: frequency histograms
├── Ранговые кривые: rank-frequency plots
├── Логарифмические графики: log-log plots
├── Облака слов: word clouds
├── Тепловые карты: heatmaps
├── Диаграммы рассеяния: scatter plots
└── Временные ряды: time series
```

### Коллокационный анализ
```
Определение коллокаций:
├── Статистическая значимость: statistical significance
├── Взаимная информация: mutual information
├── T-score: t-статистика
├── Log-likelihood: логарифм правдоподобия
├── Chi-square: хи-квадрат
├── Dice coefficient: коэффициент Дайса
├── Jaccard index: индекс Жаккара
└── PMI: pointwise mutual information

Типы коллокаций:
├── Лексические коллокации: word combinations
├── Грамматические коллокации: patterns
├── Фразовые коллокации: phrases
├── Идиоматические коллокации: idioms
├── Технические коллокации: technical terms
├── Культурные коллокации: cultural expressions
└── Временные коллокации: temporal patterns

Окна анализа:
├── Span: размер окна (обычно ±3-5 слов)
├── Позиционная чувствительность: position
├── Направленность: left/right collocates
├── Границы предложений: sentence boundaries
├── Синтаксические границы: syntactic units
├── Семантические границы: semantic units
└── Дискурсивные границы: discourse units
```

### Конкорданс-анализ
```
Типы конкордансов:
├── KWIC: Key Word In Context
├── KWOC: Key Word Out of Context
├── KWAC: Key Word And Context
├── Полный конкорданс: full concordance
├── Выборочный конкорданс: sample concordance
├── Сортированный конкорданс: sorted concordance
└── Фильтрованный конкорданс: filtered concordance

Сортировка:
├── Алфавитная сортировка: alphabetical
├── Сортировка по частоте: frequency-based
├── Левый контекст: left context
├── Правый контекст: right context
├── Позиционная сортировка: position-based
├── Случайная сортировка: random
└── Семантическая сортировка: semantic

Анализ паттернов:
├── Синтаксические паттерны: syntactic patterns
├── Семантические паттерны: semantic patterns
├── Прагматические паттерны: pragmatic patterns
├── Дискурсивные паттерны: discourse patterns
├── Стилистические паттерны: stylistic patterns
├── Регистровые паттерны: register patterns
└── Жанровые паттерны: genre patterns
```

## 📈 Статистические методы

### Описательная статистика
```
Центральные тенденции:
├── Среднее арифметическое: mean
├── Медиана: median
├── Мода: mode
├── Геометрическое среднее: geometric mean
├── Гармоническое среднее: harmonic mean
├── Усеченное среднее: trimmed mean
└── Взвешенное среднее: weighted mean

Меры разброса:
├── Стандартное отклонение: standard deviation
├── Дисперсия: variance
├── Размах: range
├── Интерквартильный размах: IQR
├── Коэффициент вариации: CV
├── Медианное абсолютное отклонение: MAD
└── Процентили: percentiles

Меры формы:
├── Асимметрия: skewness
├── Эксцесс: kurtosis
├── Момент: moment
├── Квантили: quantiles
├── Энтропия: entropy
└── Информационное содержание: information content
```

### Статистические тесты
```
Тесты на нормальность:
├── Тест Шапиро-Уилка: Shapiro-Wilk test
├── Тест Колмогорова-Смирнова: KS test
├── Тест Андерсона-Дарлинга: AD test
├── Тест Жака-Бера: Jarque-Bera test
├── Тест Лиллиефорса: Lilliefors test
├── Q-Q plots: quantile-quantile plots
└── Гистограммы: histograms

Тесты сравнения:
├── t-тест: t-test
├── Тест Манна-Уитни: Mann-Whitney U test
├── Тест Вилкоксона: Wilcoxon test
├── Тест Краскела-Уоллиса: Kruskal-Wallis test
├── ANOVA: analysis of variance
├── Хи-квадрат: chi-square test
└── Точный тест Фишера: Fisher's exact test

Тесты корреляции:
├── Корреляция Пирсона: Pearson correlation
├── Корреляция Спирмена: Spearman correlation
├── Корреляция Кендалла: Kendall correlation
├── Частичная корреляция: partial correlation
├── Множественная корреляция: multiple correlation
├── Каноническая корреляция: canonical correlation
└── Дистанционная корреляция: distance correlation
```

### Многомерный анализ
```
Снижение размерности:
├── Анализ главных компонент: PCA
├── Факторный анализ: Factor Analysis
├── Многомерное шкалирование: MDS
├── t-SNE: t-distributed Stochastic Neighbor Embedding
├── UMAP: Uniform Manifold Approximation
├── Независимые компоненты: ICA
└── Нелинейные методы: nonlinear methods

Кластерный анализ:
├── K-means: k-средних
├── Иерархический кластеринг: hierarchical clustering
├── DBSCAN: density-based clustering
├── Gaussian mixture models: GMM
├── Spectral clustering: спектральный кластеринг
├── Affinity propagation: распространение близости
└── Оценка качества: cluster validation

Классификация:
├── Логистическая регрессия: logistic regression
├── Деревья решений: decision trees
├── Случайный лес: random forest
├── Машины опорных векторов: SVM
├── Наивный байесовский классификатор: Naive Bayes
├── k-ближайших соседей: k-NN
└── Нейронные сети: neural networks
```

## 🖥️ Программное обеспечение

### Специализированные инструменты
```
AntConc:
├── Разработчик: Laurence Anthony
├── Платформа: кросс-платформенный
├── Функции: конкордансы, коллокации, частоты
├── Формат: plain text
├── Цена: бесплатно
├── Особенности: простота использования
└── Ограничения: размер корпуса

WordSmith Tools:
├── Разработчик: Mike Scott
├── Платформа: Windows
├── Функции: Concord, WordList, KeyWords
├── Формат: различные форматы
├── Цена: коммерческая
├── Особенности: мощная функциональность
└── Версии: регулярные обновления

Sketch Engine:
├── Разработчик: Lexical Computing
├── Платформа: веб-интерфейс
├── Функции: word sketches, thesaurus
├── Корпуса: предустановленные
├── Цена: подписка
├── Особенности: word sketches
└── Языки: множественные
```

### Программирование
```
Python:
├── NLTK: Natural Language Toolkit
├── spaCy: промышленная обработка
├── Gensim: тематическое моделирование
├── scikit-learn: машинное обучение
├── pandas: анализ данных
├── NumPy: численные вычисления
├── matplotlib/seaborn: визуализация
└── Jupyter: интерактивные тетради

R:
├── tm: text mining
├── quanteda: количественный анализ
├── tidytext: tidy text analysis
├── stringr: строковые операции
├── ggplot2: визуализация
├── dplyr: манипулирование данными
├── RStudio: IDE
└── R Markdown: отчеты

Другие инструменты:
├── Perl: регулярные выражения
├── Java: GATE, Stanford CoreNLP
├── C++: высокопроизводительные приложения
├── JavaScript: веб-приложения
├── SQL: базы данных
├── Unix/Linux: командная строка
└── Git: контроль версий
```

## 🎯 Области применения

### Лексикография
```
Создание словарей:
├── Отбор лексики: word selection
├── Семантический анализ: sense disambiguation
├── Примеры использования: citation examples
├── Частотная информация: frequency data
├── Стилистические пометы: register labels
├── Коллокационные данные: collocational information
└── Обновление словарей: dictionary updates

Типы словарей:
├── Толковые словари: defining dictionaries
├── Коллокационные словари: collocation dictionaries
├── Тематические словари: thematic dictionaries
├── Терминологические словари: terminological dictionaries
├── Исторические словари: historical dictionaries
├── Двуязычные словари: bilingual dictionaries
└── Электронные словари: electronic dictionaries
```

### Обучение языку
```
Разработка материалов:
├── Отбор лексики: vocabulary selection
├── Градация трудности: difficulty grading
├── Аутентичные тексты: authentic materials
├── Упражнения: exercise design
├── Тесты: test development
├── Учебные корпуса: learner corpora
└── Адаптивное обучение: adaptive learning

Анализ языка изучающих:
├── Ошибки: error analysis
├── Развитие: developmental patterns
├── Интерязык: interlanguage
├── Перенос: transfer phenomena
├── Стратегии: learning strategies
├── Индивидуальные различия: individual differences
└── Оценка прогресса: progress assessment
```

### Машинное обучение
```
Обработка естественного языка:
├── Токенизация: tokenization
├── Лемматизация: lemmatization
├── POS-тегирование: part-of-speech tagging
├── Синтаксический анализ: parsing
├── Семантический анализ: semantic analysis
├── Классификация текстов: text classification
├── Кластеризация: clustering
└── Извлечение информации: information extraction

Языковые модели:
├── N-граммы: n-gram models
├── Нейронные сети: neural networks
├── Рекуррентные сети: RNNs
├── Трансформеры: transformers
├── BERT: bidirectional representations
├── GPT: generative pre-trained transformers
└── Предобученные модели: pre-trained models
```

## 📊 Жанровый и дискурсивный анализ

### Регистровый анализ
```
Характеристики регистров:
├── Лексическая плотность: lexical density
├── Синтаксическая сложность: syntactic complexity
├── Номинализация: nominalization
├── Пассивные конструкции: passive constructions
├── Модальность: modality
├── Время: tense usage
├── Местоимения: pronoun usage
└── Дискурсивные маркеры: discourse markers

Типы регистров:
├── Академический: academic discourse
├── Разговорный: conversational discourse
├── Официальный: formal discourse
├── Неформальный: informal discourse
├── Научный: scientific discourse
├── Юридический: legal discourse
├── Медицинский: medical discourse
└── Медийный: media discourse
```

### Дискурсивный анализ
```
Структурные элементы:
├── Когезия: cohesion
├── Когерентность: coherence
├── Тематическая прогрессия: thematic progression
├── Информационная структура: information structure
├── Референция: reference
├── Анафора: anaphora
├── Катафора: cataphora
└── Эллипсис: ellipsis

Функциональные аспекты:
├── Речевые акты: speech acts
├── Вежливость: politeness
├── Импликатуры: implicatures
├── Пресуппозиции: presuppositions
├── Метадискурс: metadiscourse
├── Интертекстуальность: intertextuality
└── Мультимодальность: multimodality
```

## 🌐 Веб-корпуса и большие данные

### Сбор веб-данных
```
Методы сбора:
├── Веб-скрапинг: web scraping
├── API интерфейсы: API access
├── Поисковые системы: search engines
├── Социальные сети: social media
├── Новостные сайты: news websites
├── Блоги: blogs
├── Форумы: forums
└── Википедия: Wikipedia

Инструменты:
├── Beautiful Soup: Python library
├── Scrapy: scraping framework
├── Selenium: browser automation
├── Requests: HTTP library
├── BootCat: corpus building
├── Common Crawl: web crawl data
├── Twitter API: Twitter data
└── Reddit API: Reddit data
```

### Обработка больших данных
```
Вычислительные вызовы:
├── Объем данных: data volume
├── Скорость обработки: processing speed
├── Память: memory requirements
├── Хранение: storage needs
├── Параллельная обработка: parallel processing
├── Распределенные вычисления: distributed computing
└── Облачные вычисления: cloud computing

Технологии:
├── Hadoop: distributed storage
├── Spark: distributed processing
├── MapReduce: programming model
├── NoSQL: non-relational databases
├── MongoDB: document database
├── Elasticsearch: search engine
├── Kafka: streaming platform
└── Docker: containerization
```

### Качество данных
```
Проблемы качества:
├── Дубликаты: duplicates
├── Спам: spam content
├── Шум: noise
├── Неполнота: incompleteness
├── Ошибки: errors
├── Смещение: bias
├── Репрезентативность: representativeness
└── Этические вопросы: ethical concerns

Методы очистки:
├── Дедупликация: deduplication
├── Фильтрация: filtering
├── Нормализация: normalization
├── Проверка: validation
├── Аннотация: annotation
├── Семплирование: sampling
└── Предобработка: preprocessing
```

## 🎨 Визуализация данных

### Статические визуализации
```
Основные типы:
├── Гистограммы: histograms
├── Столбчатые диаграммы: bar charts
├── Круговые диаграммы: pie charts
├── Диаграммы рассеяния: scatter plots
├── Линейные графики: line plots
├── Коробчатые диаграммы: box plots
├── Тепловые карты: heatmaps
└── Древовидные диаграммы: tree diagrams

Специализированные:
├── Облака слов: word clouds
├── Диаграммы коллокаций: collocation networks
├── Филогенетические деревья: phylogenetic trees
├── Карты самоорганизации: self-organizing maps
├── Дендрограммы: dendrograms
├── Сетевые диаграммы: network diagrams
└── Географические карты: geographical maps
```

### Интерактивные визуализации
```
Технологии:
├── D3.js: data-driven documents
├── Plotly: interactive plots
├── Bokeh: Python visualization
├── Shiny: R web applications
├── Tableau: business intelligence
├── PowerBI: Microsoft platform
└── Observable: notebooks

Возможности:
├── Зуммирование: zooming
├── Фильтрация: filtering
├── Выделение: highlighting
├── Анимация: animation
├── Интерактивность: interactivity
├── Детализация: drill-down
└── Связывание: linking
```

## 🔬 Современные направления

### Нейронные языковые модели
```
Архитектуры:
├── Transformer: attention mechanism
├── BERT: bidirectional encoding
├── GPT: generative pre-training
├── RoBERTa: robustly optimized BERT
├── T5: text-to-text transfer
├── ELECTRA: replaced token detection
└── DeBERTa: decoding-enhanced BERT

Анализ моделей:
├── Attention weights: веса внимания
├── Probing tasks: зондирующие задачи
├── Representation analysis: анализ представлений
├── Interpretability: интерпретируемость
├── Bias detection: обнаружение смещений
├── Fairness evaluation: оценка справедливости
└── Adversarial examples: состязательные примеры
```

### Мультимодальный анализ
```
Типы модальностей:
├── Текст: text
├── Изображения: images
├── Аудио: audio
├── Видео: video
├── Жесты: gestures
├── Мимика: facial expressions
└── Движения глаз: eye movements

Методы анализа:
├── Мультимодальное слияние: multimodal fusion
├── Кросс-модальное обучение: cross-modal learning
├── Совместные представления: joint representations
├── Выравнивание модальностей: modality alignment
├── Трансфер между модальностями: modality transfer
├── Генерация: generation
└── Понимание: understanding
```

### Этические вопросы
```
Основные проблемы:
├── Приватность: privacy
├── Согласие: consent
├── Смещения: biases
├── Дискриминация: discrimination
├── Манипуляция: manipulation
├── Прозрачность: transparency
└── Ответственность: accountability

Решения:
├── Этические комитеты: ethics boards
├── Руководящие принципы: guidelines
├── Аудит алгоритмов: algorithm auditing
├── Объяснимый ИИ: explainable AI
├── Справедливые модели: fair models
├── Защита данных: data protection
└── Общественное участие: public participation
```

## 📚 Рекомендации для изучения

### Базовые навыки
- Основы статистики
- Программирование (Python/R)
- Лингвистические основы
- Методы исследования
- Критическое мышление

### Специализированные курсы
- Корпусная лингвистика
- Компьютерная лингвистика
- Анализ данных
- Машинное обучение
- Визуализация данных

### Практические проекты
- Создание собственного корпуса
- Анализ конкретных явлений
- Сравнительные исследования
- Исторические изменения
- Вариативность языка

### Рекомендуемая литература
- McEnery, T. & Hardie, A. "Corpus Linguistics"
- Baker, P. "Using Corpora in Discourse Analysis"
- Gries, S. "Quantitative Corpus Linguistics with R"
- Wynne, M. "Developing Linguistic Corpora"
- Biber, D. "Corpus Linguistics: Investigating Language Structure and Use"

### Программное обеспечение
- AntConc: бесплатный инструмент
- Python: NLTK, spaCy, scikit-learn
- R: quanteda, tm, tidytext
- Онлайн-корпуса: COCA, BNC, Google Books
- Визуализация: ggplot2, matplotlib, D3.js

### Онлайн-ресурсы
- Corpus Linguistics Course (Lancaster University)
- Text Analysis with R (DataCamp)
- Natural Language Processing (Coursera)
- Корпусные данные: различные корпуса
- GitHub: репозитории с кодом 