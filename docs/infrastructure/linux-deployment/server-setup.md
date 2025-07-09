# 🖥️ Server Setup: Подготовка Linux серверов

## Описание

Настройка производственных Linux серверов для развертывания микросервисов включает базовую конфигурацию системы, настройку безопасности, установку Docker и подготовку инфраструктуры.

## Базовая настройка Ubuntu/CentOS

### Автоматизированный скрипт настройки

```bash
#!/bin/bash
# scripts/server-setup.sh
# Автоматизированная настройка продакшн сервера для микросервисов

# ==================== ОБНОВЛЕНИЕ СИСТЕМЫ ====================
# Определение операционной системы и обновление пакетов

# Проверка наличия менеджера пакетов apt (Ubuntu/Debian)
if command -v apt &> /dev/null; then
    echo "🔄 Обнаружена система на базе Debian/Ubuntu"
    # Обновление списка пакетов и установка обновлений
    sudo apt update && sudo apt upgrade -y
    # Установка необходимых базовых пакетов
    sudo apt install -y \
        curl \                     # HTTP клиент для загрузки файлов
        wget \                     # Альтернативный HTTP клиент
        git \                      # Система контроля версий
        vim \                      # Текстовый редактор
        htop \                     # Мониторинг процессов
        unzip \                    # Архиватор
        software-properties-common # Управление репозиториями

# Проверка наличия менеджера пакетов yum (CentOS/RHEL)
elif command -v yum &> /dev/null; then
    echo "🔄 Обнаружена система на базе RedHat/CentOS"
    # Обновление всех установленных пакетов
    sudo yum update -y
    # Установка базовых пакетов и EPEL репозитория
    sudo yum install -y \
        curl \        # HTTP клиент
        wget \        # Альтернативный HTTP клиент
        git \         # Система контроля версий
        vim \         # Текстовый редактор
        htop \        # Мониторинг процессов
        unzip \       # Архиватор
        epel-release  # Дополнительный репозиторий пакетов
fi

# ==================== НАСТРОЙКА FIREWALL ====================
# Конфигурация межсетевого экрана для безопасности

echo "🔥 Настройка firewall..."
# Включение UFW (Uncomplicated Firewall)
sudo ufw enable

# Разрешение SSH соединений (порт 22)
sudo ufw allow ssh

# Разрешение HTTP трафика (порт 80)
sudo ufw allow 80

# Разрешение HTTPS трафика (порт 443)
sudo ufw allow 443

# Разрешение портов для наших микросервисов (8080-8082)
sudo ufw allow 8080:8082/tcp  # API Gateway, User Service, Order Service

# ==================== СОЗДАНИЕ ПОЛЬЗОВАТЕЛЯ ДЛЯ РАЗВЕРТЫВАНИЯ ====================
echo "👤 Создание пользователя deploy..."

# Создание пользователя с домашней директорией и bash shell
sudo useradd -m -s /bin/bash deploy

# Добавление пользователя в группу sudo для административных прав
sudo usermod -aG sudo deploy

# Создание директории для SSH ключей
sudo mkdir -p /home/deploy/.ssh

# Установка безопасных прав доступа (только владелец может читать/писать/выполнять)
sudo chmod 700 /home/deploy/.ssh

# ==================== НАСТРОЙКА SSH КЛЮЧЕЙ ====================
echo "🔑 Настройка SSH ключей..."

# Добавление публичного SSH ключа для беспарольной аутентификации
# ВАЖНО: Замените на ваш реальный публичный ключ
echo "ssh-rsa AAAAB3NzaC1yc2E... deploy@ci-server" | sudo tee /home/deploy/.ssh/authorized_keys

# Установка безопасных прав для файла с ключами (только владелец может читать/писать)
sudo chmod 600 /home/deploy/.ssh/authorized_keys

# Установка правильного владельца для всей SSH директории
sudo chown -R deploy:deploy /home/deploy/.ssh

# ==================== ПОВЫШЕНИЕ БЕЗОПАСНОСТИ SSH ====================
echo "🔒 Настройка безопасности SSH..."

# Отключение аутентификации по паролю (принудительное использование ключей)
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# Перезапуск SSH демона для применения изменений
sudo systemctl restart sshd

# ==================== НАСТРОЙКА ЛОГИРОВАНИЯ ====================
echo "📝 Настройка логирования приложений..."

# Создание директории для логов приложений
sudo mkdir -p /var/log/applications

# Установка владельца директории логов
sudo chown deploy:deploy /var/log/applications

# ==================== ЗАВЕРШЕНИЕ НАСТРОЙКИ ====================
echo "✅ Настройка сервера завершена успешно!"
echo "📋 Выполнено:"
echo "   - Обновлена система"
echo "   - Настроен firewall"
echo "   - Создан пользователь deploy"
echo "   - Настроена SSH аутентификация по ключам"
echo "   - Подготовлена директория для логов"
echo ""
echo "🚀 Сервер готов к развертыванию микросервисов!"
```

## Установка Docker на Linux

### Скрипт автоматической установки Docker

```bash
#!/bin/bash
# scripts/install-docker.sh
# Установка Docker и Docker Compose на Linux сервер

echo "🐳 Установка Docker..."

# ==================== УСТАНОВКА DOCKER ====================
# Загрузка и выполнение официального скрипта установки Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# ==================== НАСТРОЙКА ПОЛЬЗОВАТЕЛЕЙ ====================
# Добавление текущего пользователя в группу docker
sudo usermod -aG docker $USER

# Добавление пользователя deploy в группу docker
sudo usermod -aG docker deploy

# ==================== УСТАНОВКА DOCKER COMPOSE ====================
echo "📦 Установка Docker Compose..."

# Загрузка Docker Compose v2.23.0
sudo curl -L "https://github.com/docker/compose/releases/download/v2.23.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Установка прав на выполнение
sudo chmod +x /usr/local/bin/docker-compose

# ==================== НАСТРОЙКА АВТОЗАПУСКА ====================
echo "⚙️ Настройка автозапуска Docker..."

# Включение автозапуска Docker при загрузке системы
sudo systemctl enable docker

# Запуск Docker службы
sudo systemctl start docker

# ==================== ПРОВЕРКА УСТАНОВКИ ====================
echo "🔍 Проверка установки..."

# Проверка версии Docker
docker_version=$(docker --version)
echo "Docker: $docker_version"

# Проверка версии Docker Compose
compose_version=$(docker-compose --version)
echo "Docker Compose: $compose_version"

# Тест запуска контейнера
if docker run --rm hello-world > /dev/null 2>&1; then
    echo "✅ Docker установлен и работает корректно!"
else
    echo "❌ Ошибка при тестировании Docker"
    exit 1
fi

echo "🎉 Установка Docker завершена успешно!"
echo ""
echo "📝 Примечание: Для применения изменений группы docker выполните:"
echo "   newgrp docker"
echo "   или перелогиньтесь в систему"
```

## Настройка системных лимитов

### Оптимизация для высоких нагрузок

```bash
#!/bin/bash
# scripts/optimize-limits.sh
# Настройка системных лимитов для высоконагруженных приложений

echo "⚡ Настройка системных лимитов..."

# ==================== УВЕЛИЧЕНИЕ ЛИМИТОВ НА ФАЙЛЫ ====================
echo "📁 Настройка лимитов на открытые файлы..."

# Добавление лимитов в /etc/security/limits.conf
sudo tee -a /etc/security/limits.conf << 'EOF'

# Лимиты для высоконагруженных приложений
# Увеличение количества открытых файлов для всех пользователей
*               soft    nofile          65536
*               hard    nofile          65536

# Лимиты для пользователя deploy
deploy          soft    nofile          65536
deploy          hard    nofile          65536
deploy          soft    nproc           32768
deploy          hard    nproc           32768
EOF

# ==================== НАСТРОЙКА SYSTEMD ЛИМИТОВ ====================
echo "🔧 Настройка systemd лимитов..."

# Создание директории для переопределения systemd лимитов
sudo mkdir -p /etc/systemd/system.conf.d/

# Настройка глобальных лимитов systemd
sudo tee /etc/systemd/system.conf.d/limits.conf << 'EOF'
[Manager]
DefaultLimitNOFILE=65536
DefaultLimitNPROC=32768
EOF

# ==================== НАСТРОЙКА СЕТЕВЫХ ПАРАМЕТРОВ ====================
echo "🌐 Оптимизация сетевых параметров..."

# Создание файла с сетевыми оптимизациями
sudo tee /etc/sysctl.d/99-microservices.conf << 'EOF'
# Увеличение буферов сети
net.core.rmem_default = 262144
net.core.rmem_max = 16777216
net.core.wmem_default = 262144
net.core.wmem_max = 16777216

# Увеличение буферов TCP
net.ipv4.tcp_rmem = 4096 65536 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216

# Увеличение backlog для входящих соединений
net.core.netdev_max_backlog = 5000
net.ipv4.tcp_max_syn_backlog = 8192

# Быстрая переработка TIME_WAIT соединений
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 30

# Увеличение количества локальных портов
net.ipv4.ip_local_port_range = 1024 65535

# Улучшение производительности файлового кэша
vm.dirty_ratio = 15
vm.dirty_background_ratio = 5
EOF

# Применение новых параметров
sudo sysctl -p /etc/sysctl.d/99-microservices.conf

# ==================== НАСТРОЙКА DOCKER ЛИМИТОВ ====================
echo "🐳 Настройка Docker лимитов..."

# Создание директории для конфигурации Docker
sudo mkdir -p /etc/docker

# Настройка Docker daemon с оптимизированными параметрами
sudo tee /etc/docker/daemon.json << 'EOF'
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "10m",
        "max-file": "3"
    },
    "default-ulimits": {
        "nofile": {
            "name": "nofile",
            "hard": 65536,
            "soft": 65536
        },
        "nproc": {
            "name": "nproc",
            "hard": 32768,
            "soft": 32768
        }
    },
    "storage-driver": "overlay2",
    "features": {
        "buildkit": true
    }
}
EOF

# Перезапуск Docker для применения изменений
sudo systemctl restart docker

echo "✅ Системные лимиты настроены успешно!"
echo ""
echo "📊 Текущие лимиты:"
echo "   Открытые файлы: $(ulimit -n)"
echo "   Процессы: $(ulimit -u)"
echo ""
echo "🔄 Рекомендуется перезагрузить систему для полного применения изменений"
```

## Мониторинг ресурсов сервера

### Скрипт проверки состояния системы

```bash
#!/bin/bash
# scripts/system-health.sh
# Скрипт мониторинга состояния сервера

# Цвета для вывода
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

echo "🔍 Проверка состояния системы..."
echo "=================================="

# ==================== ЗАГРУЗКА СИСТЕМЫ ====================
echo -e "\n📊 ${YELLOW}Загрузка системы:${NC}"
uptime

# ==================== ИСПОЛЬЗОВАНИЕ CPU ====================
echo -e "\n💻 ${YELLOW}Использование CPU:${NC}"
top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print "CPU Usage: " 100 - $1 "%"}'

# ==================== ИСПОЛЬЗОВАНИЕ ПАМЯТИ ====================
echo -e "\n🧠 ${YELLOW}Использование памяти:${NC}"
free -h | awk '/^Mem:/ {printf "Memory Usage: %s/%s (%.2f%%)\n", $3, $2, ($3/$2)*100}'

# ==================== ИСПОЛЬЗОВАНИЕ ДИСКА ====================
echo -e "\n💾 ${YELLOW}Использование диска:${NC}"
df -h | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{print $5 " " $1 " " $6}' | while read output;
do
  usage=$(echo $output | awk '{print $1}' | sed 's/%//g')
  partition=$(echo $output | awk '{print $2}')
  mount=$(echo $output | awk '{print $3}')
  
  if [ $usage -ge 85 ]; then
    echo -e "${RED}WARNING: $mount ($partition) is ${usage}% full${NC}"
  elif [ $usage -ge 70 ]; then
    echo -e "${YELLOW}CAUTION: $mount ($partition) is ${usage}% full${NC}"
  else
    echo -e "${GREEN}OK: $mount ($partition) is ${usage}% full${NC}"
  fi
done

# ==================== СТАТУС DOCKER ====================
echo -e "\n🐳 ${YELLOW}Статус Docker:${NC}"
if systemctl is-active --quiet docker; then
    echo -e "${GREEN}Docker активен${NC}"
    docker system df
    echo ""
    echo "Запущенные контейнеры:"
    docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}"
else
    echo -e "${RED}Docker не активен${NC}"
fi

# ==================== СЕТЕВЫЕ СОЕДИНЕНИЯ ====================
echo -e "\n🌐 ${YELLOW}Сетевые соединения:${NC}"
ss -tuln | grep :80 > /dev/null && echo -e "${GREEN}HTTP (80) открыт${NC}" || echo -e "${RED}HTTP (80) закрыт${NC}"
ss -tuln | grep :443 > /dev/null && echo -e "${GREEN}HTTPS (443) открыт${NC}" || echo -e "${RED}HTTPS (443) закрыт${NC}"
ss -tuln | grep :8080 > /dev/null && echo -e "${GREEN}API Gateway (8080) открыт${NC}" || echo -e "${RED}API Gateway (8080) закрыт${NC}"

# ==================== ПРОВЕРКА ЛОГОВ ====================
echo -e "\n📝 ${YELLOW}Последние критические ошибки:${NC}"
journalctl --priority=err --since="1 hour ago" --no-pager -n 5

echo -e "\n✅ Проверка системы завершена"
```

## Проверочные вопросы

1. **Какие основные компоненты устанавливаются при базовой настройке сервера?**
2. **Почему важно отключить парольную аутентификацию SSH?**
3. **Какие порты должны быть открыты в firewall для микросервисов?**
4. **Как проверить статус Docker службы?**
5. **Какие системные лимиты важно настроить для высоконагруженных приложений?**

## Практические задания

### Задание 1: Базовая настройка
- Установите и настройте Ubuntu 22.04 сервер
- Выполните скрипт `server-setup.sh`
- Проверьте все настройки безопасности

### Задание 2: Docker установка
- Установите Docker используя автоматический скрипт
- Настройте Docker для работы без sudo
- Протестируйте запуск контейнера

### Задание 3: Мониторинг
- Создайте скрипт мониторинга состояния сервера
- Настройте алерты при превышении лимитов
- Интегрируйте с системой логирования

## Связанные темы

- [[ansible-automation]] - Автоматизация с Ansible
- [[nginx-load-balancer]] - Настройка балансировщика
- [[monitoring-logging]] - Мониторинг и логирование
- [[automation-scripts]] - Скрипты автоматизации 