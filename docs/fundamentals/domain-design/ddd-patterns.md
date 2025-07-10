# Domain-Driven Design (DDD)

**Domain-Driven Design** (предметно-ориентированное проектирование) — это подход к разработке программного обеспечения, который ставит в центр внимания предметную область (domain) и её логику. Концепция была предложена Эриком Эвансом в книге "Domain-Driven Design: Tackling Complexity in the Heart of Software" (2003).

## Основные концепции

### Предметная область (Domain)
**Предметная область** — это сфера деятельности, для которой разрабатывается программное обеспечение. Она включает в себя:
- Бизнес-процессы
- Правила и ограничения
- Терминологию
- Взаимодействия между сущностями

### Модель предметной области (Domain Model)
**Модель предметной области** — это абстрактное представление реальной предметной области, которое отражает её ключевые аспекты в коде.

```
┌─────────────────┐
│   Real World    │
│   Domain        │
└─────────────────┘
          │
          ▼ (abstraction)
┌─────────────────┐
│   Domain Model  │
│   (Code)        │
└─────────────────┘
```

## Строительные блоки DDD

### 1. Entity (Сущность)

**Сущность** — это объект, который имеет уникальную идентичность и жизненный цикл.

**Характеристики:**
- Имеет уникальный идентификатор
- Идентичность сохраняется в течение всего жизненного цикла
- Может изменять атрибуты, но остается той же сущностью

**Применение в проекте:**
```go
// services/user-service/internal/domain/user.go
type User struct {
    id        uuid.UUID     // Уникальный идентификатор
    email     string
    name      string
    status    UserStatus
    createdAt time.Time
    updatedAt time.Time
}

func (u *User) ID() uuid.UUID {
    return u.id
}

func (u *User) Block() error {
    if u.status == UserStatusBlocked {
        return errors.New("user is already blocked")
    }
    u.status = UserStatusBlocked
    u.updatedAt = time.Now()
    return nil
}
```

**Принципы работы с Entity:**
- Сравнение по идентификатору, а не по значению
- Инкапсуляция бизнес-логики внутри сущности
- Контроль изменений через методы

### 2. Value Object (Объект-значение)

**Объект-значение** — это объект, который описывается только своими атрибутами и не имеет концептуальной идентичности.

**Характеристики:**
- Неизменяемый (immutable)
- Сравнение по значению
- Может использоваться многократно

**Применение в проекте:**
```go
// services/user-service/internal/domain/user.go
type UserStatus int

const (
    UserStatusActive UserStatus = iota
    UserStatusBlocked
    UserStatusDeleted
)

func (s UserStatus) String() string {
    switch s {
    case UserStatusActive:
        return "active"
    case UserStatusBlocked:
        return "blocked"
    case UserStatusDeleted:
        return "deleted"
    default:
        return "unknown"
    }
}
```

```python
# services/order-service/domain/order.py
class OrderStatus:
    PENDING = "pending"
    CONFIRMED = "confirmed"
    SHIPPED = "shipped"
    DELIVERED = "delivered"
    CANCELLED = "cancelled"

class OrderItem:
    def __init__(self, product_id: str, quantity: int, price: float):
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        if price < 0:
            raise ValueError("Price cannot be negative")
        
        self.product_id = product_id
        self.quantity = quantity
        self.price = price
    
    def total_price(self) -> float:
        return self.quantity * self.price
```

### 3. Aggregate (Агрегат)

**Агрегат** — это кластер связанных объектов, которые рассматриваются как единое целое для изменения данных.

**Характеристики:**
- Имеет корень агрегата (Aggregate Root)
- Обеспечивает согласованность данных
- Граница транзакций

**Структура:**
```
┌─────────────────────────────────────┐
│            Aggregate                │
│  ┌─────────────────┐                │
│  │ Aggregate Root  │ ←── Внешний доступ
│  │   (Entity)      │                │
│  └─────────────────┘                │
│           │                         │
│           ▼                         │
│  ┌─────────────────┐                │
│  │     Entity      │                │
│  └─────────────────┘                │
│           │                         │
│           ▼                         │
│  ┌─────────────────┐                │
│  │  Value Object   │                │
│  └─────────────────┘                │
└─────────────────────────────────────┘
```

**Применение в проекте:**
```python
# services/order-service/domain/order.py
class Order:  # Aggregate Root
    def __init__(self, order_id: str, user_id: str):
        self.order_id = order_id
        self.user_id = user_id
        self.items = []  # Коллекция entities внутри агрегата
        self.status = OrderStatus.PENDING
        self.total_amount = 0.0
        self.created_at = datetime.utcnow()
        self.updated_at = datetime.utcnow()
    
    def add_item(self, product_id: str, quantity: int, price: float):
        """Бизнес-логика добавления товара"""
        if self.status != OrderStatus.PENDING:
            raise ValueError("Cannot add items to non-pending order")
        
        item = OrderItem(product_id, quantity, price)
        self.items.append(item)
        self.total_amount += item.total_price()
        self.updated_at = datetime.utcnow()
    
    def confirm(self):
        """Бизнес-логика подтверждения заказа"""
        if not self.items:
            raise ValueError("Cannot confirm empty order")
        if self.status != OrderStatus.PENDING:
            raise ValueError("Order is not in pending status")
        
        self.status = OrderStatus.CONFIRMED
        self.updated_at = datetime.utcnow()
```

**Правила агрегатов:**
- Внешние объекты могут ссылаться только на корень агрегата
- Корень агрегата контролирует доступ к внутренним объектам
- Инварианты агрегата поддерживаются при любом изменении

### 4. Domain Service (Доменный сервис)

**Доменный сервис** — это сервис, который инкапсулирует бизнес-логику, не принадлежащую конкретной сущности или объекту-значению.

**Когда использовать:**
- Операция не принадлежит естественно ни одной сущности
- Операция требует взаимодействия нескольких сущностей
- Операция является частью предметной области

**Применение в проекте:**
```go
// services/user-service/internal/domain/user.go
type UserService struct {
    userRepo UserRepository
}

func (s *UserService) IsEmailUnique(email string) (bool, error) {
    existingUser, err := s.userRepo.FindByEmail(email)
    if err != nil {
        return false, err
    }
    return existingUser == nil, nil
}
```

### 5. Repository (Репозиторий)

**Репозиторий** — это абстракция для доступа к коллекции агрегатов.

**Принципы:**
- Один репозиторий на агрегат
- Интерфейс в доменном слое
- Реализация в инфраструктурном слое

**Применение в проекте:**
```go
// services/user-service/internal/domain/user.go
type UserRepository interface {
    Save(user *User) error
    FindByID(id uuid.UUID) (*User, error)
    FindByEmail(email string) (*User, error)
    FindAll() ([]*User, error)
    Delete(id uuid.UUID) error
}
```

### 6. Factory (Фабрика)

**Фабрика** — это механизм создания сложных объектов и агрегатов.

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

## Архитектурные слои DDD

### 1. Domain Layer (Доменный слой)
- **Entities** - сущности предметной области
- **Value Objects** - объекты-значения
- **Domain Services** - доменные сервисы
- **Repository Interfaces** - интерфейсы репозиториев

### 2. Application Layer (Слой приложения)
- **Application Services** - сервисы приложения
- **Command/Query Handlers** - обработчики команд и запросов
- **DTOs** - объекты передачи данных

### 3. Infrastructure Layer (Инфраструктурный слой)
- **Repository Implementations** - реализации репозиториев
- **Database Access** - доступ к базе данных
- **External Services** - внешние сервисы

### 4. Presentation Layer (Слой представления)
- **Controllers** - контроллеры
- **Web APIs** - веб-API
- **User Interfaces** - пользовательские интерфейсы

**Диаграмма слоев:**
```
┌─────────────────────────┐
│   Presentation Layer    │
├─────────────────────────┤
│   Application Layer     │
├─────────────────────────┤
│      Domain Layer       │ ←── Core Business Logic
├─────────────────────────┤
│   Infrastructure Layer  │
└─────────────────────────┘
```

## Ubiquitous Language (Единый язык)

**Единый язык** — это общий язык, используемый всеми участниками проекта для описания предметной области.

**Принципы:**
- Используется и в коде, и в документации
- Разрабатывается совместно с экспертами предметной области
- Постоянно уточняется и развивается

**Пример из проекта:**
```go
// Терминология пользователей
type User struct {
    // "User" - пользователь системы
    // "Block" - заблокировать пользователя
    // "Active" - активный пользователь
}

func (u *User) Block() error {
    // Метод называется так, как это понимает бизнес
}
```

## Bounded Context (Ограниченный контекст)

**Ограниченный контекст** — это граница, внутри которой модель предметной области имеет определенное значение.

**Характеристики:**
- Определяет границы применимости модели
- Внутри контекста термины имеют четкое значение
- Разные контексты могут иметь разные модели для одних и тех же понятий

**Применение в проекте:**
```
┌─────────────────────┐    ┌─────────────────────┐
│   User Context      │    │   Order Context     │
│                     │    │                     │
│   User (entity)     │    │   User (value obj)  │
│   - id              │    │   - id              │
│   - email           │    │   - name            │
│   - name            │    │                     │
│   - status          │    │   Order (entity)    │
│   - createdAt       │    │   - id              │
│   - updatedAt       │    │   - user_id         │
│                     │    │   - items           │
└─────────────────────┘    └─────────────────────┘
```

## Anti-Corruption Layer (Слой защиты от повреждений)

**Слой защиты от повреждений** — это изолирующий слой, который предотвращает проникновение концепций из внешних систем в нашу доменную модель.

**Применение в проекте:**
```go
// services/user-service/internal/infrastructure/repository.go
func (r *PostgresUserRepository) Save(user *domain.User) error {
    // Преобразование доменной модели в модель базы данных
    dbUser := &UserModel{
        ID:        user.ID().String(),
        Email:     user.Email(),
        Name:      user.Name(),
        Status:    user.Status().String(),
        CreatedAt: user.CreatedAt(),
        UpdatedAt: user.UpdatedAt(),
    }
    
    // Сохранение в БД
    return r.db.Create(dbUser).Error
}
```

## Event-Driven Architecture в DDD

### Domain Events (Доменные события)

**Доменные события** — это события, которые происходят в предметной области и имеют значение для бизнеса.

**Применение в проекте:**
```go
// services/user-service/internal/domain/user.go
type UserCreatedEvent struct {
    UserID    uuid.UUID
    Email     string
    Name      string
    Timestamp time.Time
}

type UserBlockedEvent struct {
    UserID    uuid.UUID
    Reason    string
    Timestamp time.Time
}
```

## Преимущества DDD

### 1. Фокус на бизнес-логике
- Код отражает реальные бизнес-процессы
- Легче понимать и поддерживать
- Сокращается разрыв между бизнесом и техникой

### 2. Гибкость и расширяемость
- Изменения в бизнес-логике локализованы
- Новые требования легче интегрировать
- Код менее подвержен техническому долгу

### 3. Тестируемость
- Бизнес-логика изолирована от инфраструктуры
- Можно тестировать доменную логику независимо
- Четкие границы компонентов

## Недостатки DDD

### 1. Сложность
- Требует глубокого понимания предметной области
- Может быть избыточным для простых приложений
- Высокий порог входа

### 2. Время и ресурсы
- Требует больше времени на дизайн
- Необходимо тесное сотрудничество с экспертами
- Может замедлить начальную разработку

### 3. Риск избыточной абстракции
- Может привести к излишне сложным решениям
- Риск создания абстракций "на всякий случай"
- Баланс между гибкостью и простотой

## Применение DDD в микросервисах

### 1. Bounded Context как граница сервиса
```
┌─────────────────────┐    ┌─────────────────────┐
│   User Service      │    │   Order Service     │
│  (User Context)     │    │  (Order Context)    │
│                     │    │                     │
│   User Aggregate    │    │   Order Aggregate   │
│   UserRepository    │    │   OrderRepository   │
│   UserService       │    │   OrderService      │
└─────────────────────┘    └─────────────────────┘
```

### 2. Event-Driven Communication
```go
// Публикация события при создании пользователя
event := domain.UserCreatedEvent{
    UserID:    user.ID(),
    Email:     user.Email(),
    Name:      user.Name(),
    Timestamp: user.CreatedAt(),
}

eventBus.Publish("user.created", event)
```

### 3. Aggregate как граница транзакции
- Каждый агрегат обрабатывается в отдельной транзакции
- Согласованность между агрегатами через события
- Eventual consistency между сервисами

## Заключение

Domain-Driven Design предоставляет мощный инструментарий для создания сложных бизнес-приложений. Основные принципы DDD:

1. **Фокус на предметной области** - код должен отражать бизнес-логику
2. **Единый язык** - используйте термины предметной области в коде
3. **Четкие границы** - определите bounded contexts
4. **Инкапсуляция** - бизнес-логика внутри доменных объектов
5. **Изоляция** - доменный слой независим от инфраструктуры

DDD особенно эффективен в микросервисной архитектуре, где bounded contexts становятся границами сервисов, а domain events обеспечивают слабую связанность между компонентами.

---

## 🔗 Связанные темы

### ⬅️ Предыдущие шаги
- **[GoF Patterns](gof-patterns.md)** - Основные паттерны проектирования для DDD

### ➡️ Следующие шаги
После изучения DDD переходите к:
- **[CQRS Pattern](cqrs-pattern.md)** - Архитектурное применение DDD
- **[Event Sourcing](event-sourcing.md)** - Событийная архитектура для DDD
- **[Microservices Architecture](../architecture/microservices-architecture.md)** - DDD в микросервисах

### 🏛️ Архитектурное применение
- **[Microservices Architecture](../architecture/microservices-architecture.md)** - Bounded Context как граница сервиса
- **[API Design](../architecture/api-design.md)** - Проектирование API с DDD
- **[Senior Architecture](../architecture/senior-architecture.md)** - Архитектурные решения с DDD

### ⚙️ Технические навыки
- **[Databases](../technical-skills/databases.md)** - Проектирование БД с DDD
- **[Testing](../technical-skills/testing.md)** - Тестирование доменной логики
- **[Senior Technical Mastery](../technical-skills/senior-technical-mastery.md)** - Мастерство DDD

### 🚀 Практическое применение
- **[CI/CD & DevOps](../infrastructure/cicd-devops.md)** - DDD в процессах разработки
- **[Frontend Advanced](../frontend/frontend-advanced.md)** - Domain-driven frontend

### 👥 Лидерство
- **[Senior Business](../leadership/senior-business.md)** - Бизнес-понимание через DDD
- **[Senior Communication](../leadership/senior-communication.md)** - Обсуждение DDD с бизнесом

---

**Путь обучения**: [GoF Patterns](gof-patterns.md) → DDD Patterns → [CQRS Pattern](cqrs-pattern.md)  
**Сложность**: ⭐⭐⭐⭐ (4/5)  
**Время изучения**: 3-4 недели  
**Практика**: `user-service/domain/`, `order-service/domain/`