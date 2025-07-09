# Паттерны проектирования Gang of Four (GoF)

**Паттерны проектирования Gang of Four** — это 23 классических паттерна объектно-ориентированного программирования, описанные в знаменитой книге "Design Patterns: Elements of Reusable Object-Oriented Software" четырьмя авторами (Gang of Four): Эрихом Гаммой, Ричардом Хелмом, Ральфом Джонсоном и Джоном Влиссидесом в 1994 году.

## Классификация паттернов

### Порождающие паттерны (Creational Patterns)
Паттерны, которые решают проблемы создания объектов.

#### 1. Singleton (Одиночка)

**Назначение:** Обеспечивает создание единственного экземпляра класса и предоставляет глобальную точку доступа к нему.

**Структура:**
```
┌─────────────────┐
│    Singleton    │
├─────────────────┤
│ - instance      │
├─────────────────┤
│ + getInstance() │
│ - Singleton()   │
└─────────────────┘
```

**Применение в проекте:**
```go
// services/user-service/internal/infrastructure/repository.go
var dbInstance *DatabaseConnection

func GetDatabaseConnection(connectionString string) (*DatabaseConnection, error) {
    if dbInstance == nil {
        db, err := sql.Open("postgres", connectionString)
        if err != nil {
            return nil, err
        }
        dbInstance = &DatabaseConnection{db: db}
    }
    return dbInstance, nil
}
```

**Преимущества:**
- Контролируемый доступ к единственному экземпляру
- Экономия памяти
- Гибкость настройки

**Недостатки:**
- Нарушает принцип единственной ответственности
- Затрудняет модульное тестирование
- Проблемы в многопоточной среде

#### 2. Factory Method (Фабричный метод)

**Назначение:** Создает объекты без указания их конкретных классов, делегируя создание подклассам.

**Структура:**
```
    ┌─────────────┐
    │   Creator   │
    ├─────────────┤
    │ factoryMethod() │ ←── Абстрактный метод
    │ operation() │
    └─────────────┘
           ↑
    ┌─────────────┐
    │ConcreteCreator│
    ├─────────────┤
    │ factoryMethod() │ ←── Создает ConcreteProduct
    └─────────────┘
```

**Применение в проекте:**
```go
// services/user-service/internal/domain/user.go
func NewUser(email, name string) (*User, error) {
    if email == "" {
        return nil, errors.New("email cannot be empty")
    }
    if name == "" {
        return nil, errors.New("name cannot be empty")
    }
    
    user := &User{
        id:        uuid.New(),
        email:     email,
        name:      name,
        status:    UserStatusActive,
        createdAt: time.Now(),
        updatedAt: time.Now(),
    }
    return user, nil
}
```

**Преимущества:**
- Устраняет привязку к конкретным классам
- Следует принципу открытости/закрытости
- Концентрирует логику создания

### Структурные паттерны (Structural Patterns)
Паттерны, которые решают проблемы композиции классов и объектов.

#### 3. Proxy (Заместитель)

**Назначение:** Предоставляет объект-заместитель, который контролирует доступ к другому объекту.

**Применение в проекте:**
```javascript
// services/api-gateway/src/graphql/schema.js
const createUserResolvers = (serviceDiscovery) => ({
  Query: {
    user: async (_, { id }) => {
      try {
        const service = await serviceDiscovery.getService('user-service');
        const response = await axios.get(`${service.url}/api/v1/users/${id}`);
        return response.data;
      } catch (error) {
        throw new Error(`Failed to fetch user: ${error.message}`);
      }
    }
  }
});
```

### Поведенческие паттерны (Behavioral Patterns)
Паттерны, которые решают проблемы эффективного взаимодействия между объектами.

#### 4. Observer (Наблюдатель)

**Назначение:** Определяет зависимость "один ко многим" между объектами так, что при изменении состояния одного объекта все зависящие от него оповещаются автоматически.

**Структура:**
```
┌─────────────┐    наблюдает    ┌─────────────┐
│  Observer   │◄────────────────│  Subject    │
├─────────────┤                 ├─────────────┤
│ + update()  │                 │ + attach()  │
└─────────────┘                 │ + detach()  │
                                │ + notify()  │
                                └─────────────┘
```

**Применение в проекте:**
```go
// services/user-service/internal/infrastructure/repository.go
type RepositoryObserver interface {
    OnUserSaved(user *domain.User)
    OnUserDeleted(userID uuid.UUID)
}

func (r *PostgresUserRepository) AddObserver(observer RepositoryObserver) {
    r.observers = append(r.observers, observer)
}

func (r *PostgresUserRepository) notifyUserSaved(user *domain.User) {
    for _, observer := range r.observers {
        observer.OnUserSaved(user)
    }
}
```

**Преимущества:**
- Слабая связанность между субъектом и наблюдателями
- Поддержка широковещательных коммуникаций
- Возможность динамически добавлять/удалять наблюдателей

#### 5. Command (Команда)

**Назначение:** Инкапсулирует запрос как объект, позволяя параметризовать клиентов с различными запросами, ставить запросы в очередь и поддерживать отмену операций.

**Структура:**
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────►│  Invoker    │────►│  Command    │
└─────────────┘     ├─────────────┤     ├─────────────┤
                    │ + setCommand│     │ + execute() │
                    │ + execute() │     └─────────────┘
                    └─────────────┘            │
                                               ▼
                                    ┌─────────────┐
                                    │  Receiver   │
                                    ├─────────────┤
                                    │ + action()  │
                                    └─────────────┘
```

**Применение в проекте:**
```go
// services/user-service/internal/application/commands.go
type Command interface {
    Execute() error
}

type CreateUserCommand struct {
    Email string
    Name  string
    userRepo domain.UserRepository
    eventBus EventBus
}

func (c *CreateUserCommand) Execute() error {
    user, err := domain.NewUser(c.Email, c.Name)
    if err != nil {
        return err
    }
    
    if err := c.userRepo.Save(user); err != nil {
        return err
    }
    
    event := domain.UserCreatedEvent{
        UserID:    user.ID(),
        Email:     user.Email(),
        Name:      user.Name(),
        Timestamp: user.CreatedAt(),
    }
    
    return c.eventBus.Publish("user.created", event)
}
```

**Преимущества:**
- Разделяет объект, инициирующий операцию, от объекта, который её выполняет
- Позволяет параметризовать объекты с различными запросами
- Поддерживает отмену операций (Undo)
- Позволяет ставить запросы в очередь

#### 6. Strategy (Стратегия)

**Назначение:** Определяет семейство алгоритмов, инкапсулирует каждый из них и делает их взаимозаменяемыми.

**Применение в проекте:**
```go
// services/user-service/internal/application/commands.go
func (h *CommandHandler) Handle(cmd Command) error {
    return cmd.Execute()  // Стратегия выполнения зависит от типа команды
}
```

## Принципы проектирования

### SOLID принципы
1. **Single Responsibility Principle (SRP)** - Каждый класс должен иметь только одну причину для изменения
2. **Open/Closed Principle (OCP)** - Классы должны быть открыты для расширения, но закрыты для модификации
3. **Liskov Substitution Principle (LSP)** - Объекты должны быть заменяемыми экземплярами своих подтипов
4. **Interface Segregation Principle (ISP)** - Клиенты не должны зависеть от интерфейсов, которые они не используют
5. **Dependency Inversion Principle (DIP)** - Зависеть от абстракций, а не от конкретных реализаций

### Дополнительные принципы
- **DRY (Don't Repeat Yourself)** - Не повторяйся
- **KISS (Keep It Simple, Stupid)** - Делай проще
- **YAGNI (You Aren't Gonna Need It)** - Вам это не понадобится

## Применение в современной разработке

### В микросервисах
- **Factory Method** для создания сервисов
- **Observer** для event-driven архитектуры
- **Command** для CQRS паттерна
- **Proxy** для API Gateway
- **Singleton** для подключений к БД

### В веб-разработке
- **MVC как Strategy** - различные стратегии обработки запросов
- **Observer** для reactive programming
- **Command** для undo/redo функциональности

## Антипаттерны

### God Object
Класс, который знает или делает слишком много.

### Spaghetti Code
Код с плохой структурой и запутанной логикой.

### Copy-Paste Programming
Дублирование кода вместо создания переиспользуемых компонентов.

## Заключение

Паттерны GoF остаются актуальными и сегодня, предоставляя проверенные решения для типичных проблем проектирования. Важно помнить, что паттерны - это инструменты, а не самоцель. Их нужно применять обдуманно, когда они действительно решают проблему, а не усложняют код.

---

## 🔗 Связанные темы

### ➡️ Следующие шаги
После изучения GoF паттернов переходите к:
- **[DDD Patterns](ddd-patterns.md)** - Применение паттернов в доменном моделировании
- **[CQRS Pattern](cqrs-pattern.md)** - Command паттерн в архитектуре
- **[Event Sourcing](event-sourcing.md)** - Observer паттерн в событийном моделировании

### 🏛️ Архитектурное применение
- **[Microservices Architecture](../architecture/microservices-architecture.md)** - Паттерны в микросервисной архитектуре
- **[API Design](../architecture/api-design.md)** - Proxy и Facade паттерны в API
- **[Senior Architecture](../architecture/senior-architecture.md)** - Архитектурные решения с паттернами

### ⚙️ Технические навыки
- **[Concurrency & Async](../technical-skills/concurrency-async.md)** - Singleton и Observer в многопоточности
- **[Testing](../technical-skills/testing.md)** - Тестирование паттернов
- **[Senior Technical Mastery](../technical-skills/senior-technical-mastery.md)** - Мастерство применения паттернов

### 🚀 Практическое применение
- **[CI/CD & DevOps](../infrastructure/cicd-devops.md)** - Builder и Factory паттерны в CI/CD
- **[Frontend Advanced](../frontend/frontend-advanced.md)** - Паттерны в React (Observer, Factory)

---

**Путь обучения**: [Fundamentals Overview](README.md) → GoF Patterns → [DDD Patterns](ddd-patterns.md)  
**Сложность**: ⭐⭐⭐ (3/5)  
**Время изучения**: 2-3 недели  
**Практика**: `user-service/`, `api-gateway/`, `order-service/`