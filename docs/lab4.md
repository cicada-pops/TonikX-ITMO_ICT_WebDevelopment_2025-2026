# Лабораторная работа 4: Реализация клиентской части приложения средствами Vue.js

В этой работе реализована клиентская часть системы управления издательской деятельностью с использованием Vue 3 и Vuetify 3.

## Техническое задание

**Цель:** Реализовать клиентское приложение для серверной части из лабораторной работы №3.

**Требуемый функционал:**

- Настройка CORS для взаимодействия frontend ↔ backend
- Интерфейсы авторизации, регистрации и управления профилем
- Клиентские интерфейсы для всех сущностей системы
- Подключение Vuetify или аналогичной библиотеки

---

## 1. Настройка CORS на серверной части

Для корректного взаимодействия клиентского приложения с сервером была выполнена настройка CORS:

### Установка зависимости

```bash
pip install django-cors-headers
```

### Конфигурация settings.py

```python
INSTALLED_APPS = [
    ...
    'corsheaders',
    ...
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',  # Добавлено
    'django.middleware.common.CommonMiddleware',
    ...
]

# CORS settings
CORS_ALLOWED_ORIGINS = [
    'http://localhost:5173',
    'http://127.0.0.1:5173',
    'http://localhost:3000',
    'http://127.0.0.1:3000',
]

CORS_ALLOW_CREDENTIALS = True

CORS_ALLOW_HEADERS = [
    'accept',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
]
```

---

## 2. Архитектура клиентского приложения

### Используемые технологии

| Технология | Версия | Назначение |
|------------|--------|------------|
| Vue.js | 3.4.21 | JavaScript фреймворк |
| Vuetify | 3.5.11 | UI компоненты Material Design |
| Vue Router | 4.3.0 | Маршрутизация SPA |
| Pinia | 2.1.7 | Управление состоянием |
| Axios | 1.6.8 | HTTP клиент |
| Vite | 5.2.6 | Сборщик проекта |

### Структура проекта

```
publishing_frontend/
├── src/
│   ├── views/              # Страницы приложения
│   │   ├── LoginView.vue
│   │   ├── RegisterView.vue
│   │   ├── ProfileView.vue
│   │   ├── HomeView.vue
│   │   ├── EmployeesView.vue
│   │   ├── AuthorsView.vue
│   │   ├── BooksView.vue
│   │   ├── BookDetailView.vue
│   │   ├── ContractsView.vue
│   │   ├── CustomersView.vue
│   │   ├── OrdersView.vue
│   │   ├── OrderDetailView.vue
│   │   ├── ReportsView.vue
│   │   └── NotFoundView.vue
│   ├── stores/             # Pinia хранилища
│   │   └── auth.js
│   ├── services/           # API сервисы
│   │   └── api.js
│   ├── router/             # Маршрутизация
│   │   └── index.js
│   ├── App.vue             # Корневой компонент
│   └── main.js             # Точка входа
├── package.json
├── vite.config.js
└── index.html
```

---

## 3. Реализация интерфейсов авторизации и регистрации

### 3.1. Страница входа (LoginView)

Реализована форма входа с валидацией:

- Поля: имя пользователя, пароль
- Отображение/скрытие пароля
- Обработка ошибок авторизации
- Перенаправление на главную после входа

**Особенности дизайна:**
- Анимированный градиентный фон
- Центрированная карточка с формой
- Иконка приложения
- Ссылка на регистрацию

### 3.2. Страница регистрации (RegisterView)

- Поля: имя пользователя, email, пароль, подтверждение пароля
- Валидация формата email
- Проверка совпадения паролей
- Минимальная длина пароля (8 символов)
- Сообщение об успешной регистрации

### 3.3. Управление профилем (ProfileView)

Две формы на странице:

**Информация профиля:**
- Изменение имени пользователя
- Изменение email
- Изменение имени и фамилии

**Смена пароля:**
- Текущий пароль
- Новый пароль
- Подтверждение нового пароля

---

## 4. Реализация клиентских интерфейсов

### 4.1. Главная страница (Dashboard)

Панель быстрого доступа с карточками для всех разделов:

- Сотрудники
- Авторы
- Книги
- Контракты
- Заказчики
- Заказы
- Отчёты

Каждая карточка имеет:
- Уникальную иконку
- Градиентный фон
- Анимацию при наведении

### 4.2. Управление сотрудниками

**Функционал:**
- Таблица с пагинацией
- Поиск по ФИО, email, должности
- Фильтрация по роли
- Создание/редактирование/удаление

**Поля формы:**
- Фамилия, имя, отчество
- Email, телефон
- Роль (Менеджер/Редактор/Другое)
- Должность
- Дата приёма

### 4.3. Управление авторами

**Функционал:**
- Таблица авторов
- Поиск по имени
- Отображение количества книг
- CRUD операции

**Поля формы:**
- ФИО
- Email, телефон
- Дата рождения
- Биография

### 4.4. Управление книгами

**Функционал:**
- Таблица с фильтрацией
- Фильтр по жанру
- Фильтр по наличию иллюстраций
- Переход к детальной странице

**Детальная страница книги:**
- Полная информация о книге
- Список авторов с порядком и процентом гонорара
- Список редакторов с указанием ответственного
- Информация о контракте
- Добавление/удаление авторов и редакторов

### 4.5. Управление контрактами

**Функционал:**
- Фильтрация по статусу
- Выбор книги из списка доступных
- Выбор менеджера
- Валидация аванса и бюджета

**Статусы контрактов:**
- Черновик
- Активный
- Завершён
- Расторгнут

### 4.6. Управление заказчиками

**Поля:**
- Имя
- Название компании
- Email, телефон
- Адрес

### 4.7. Управление заказами

**Функционал:**
- Создание заказа
- Фильтрация по статусу
- Детальная страница с позициями

**Детальная страница заказа:**
- Информация о заказчике
- Список позиций с количеством и ценой
- Добавление новых позиций
- Автоматический расчёт суммы

---

## 5. Реализация интерфейсов отчётов

Страница отчётов реализована с использованием вкладок (tabs):

### 5.1. Книги по автору
- Выбор автора из выпадающего списка
- Таблица книг выбранного автора
- Информация о контракте

### 5.2. Ответственные редакторы
- Список всех книг с ответственными редакторами
- ISBN книги
- Контактная информация редактора

### 5.3. Количество редакторов
- Таблица книг
- Количество редакторов на каждую книгу
- Имя ответственного редактора

### 5.4. Контракты по месяцам
- Выбор года
- Таблица с количеством контрактов по месяцам
- Общее количество за год

### 5.5. Топ менеджеры
- Выбор периода (дата начала и конца)
- Список менеджеров с максимальным количеством контрактов
- Контактная информация

### 5.6. Квартальный отчёт
- Выбор квартала и года
- Итоги по месяцам
- Детальная таблица контрактов:
  - Название книги
  - Количество авторов и редакторов
  - Количество страниц
  - Наличие иллюстраций

---

## 6. Навигация и маршрутизация

### Vue Router конфигурация

```javascript
const routes = [
  { path: '/login', name: 'login', meta: { requiresGuest: true } },
  { path: '/register', name: 'register', meta: { requiresGuest: true } },
  { path: '/', name: 'home', meta: { requiresAuth: true } },
  { path: '/profile', name: 'profile', meta: { requiresAuth: true } },
  { path: '/employees', name: 'employees', meta: { requiresAuth: true } },
  { path: '/authors', name: 'authors', meta: { requiresAuth: true } },
  { path: '/books', name: 'books', meta: { requiresAuth: true } },
  { path: '/books/:id', name: 'book-detail', meta: { requiresAuth: true } },
  { path: '/contracts', name: 'contracts', meta: { requiresAuth: true } },
  { path: '/customers', name: 'customers', meta: { requiresAuth: true } },
  { path: '/orders', name: 'orders', meta: { requiresAuth: true } },
  { path: '/orders/:id', name: 'order-detail', meta: { requiresAuth: true } },
  { path: '/reports', name: 'reports', meta: { requiresAuth: true } },
]
```

### Navigation Guards

```javascript
router.beforeEach((to, from, next) => {
  const authStore = useAuthStore()

  if (to.meta.requiresAuth && !authStore.isAuthenticated) {
    next({ name: 'login', query: { redirect: to.fullPath } })
  } else if (to.meta.requiresGuest && authStore.isAuthenticated) {
    next({ name: 'home' })
  } else {
    next()
  }
})
```

---

## 7. Управление состоянием (Pinia)

### Auth Store

```javascript
export const useAuthStore = defineStore('auth', {
  state: () => ({
    user: null,
    token: localStorage.getItem('auth_token') || null,
    loading: false,
    error: null,
  }),

  getters: {
    isAuthenticated: (state) => !!state.token,
  },

  actions: {
    async login(credentials) { ... },
    async register(userData) { ... },
    async logout() { ... },
    async fetchProfile() { ... },
    async updateProfile(data) { ... },
    async changePassword(data) { ... },
  }
})
```

---

## 8. API Сервис

Централизованный модуль для работы с API:

```javascript
const api = axios.create({
  baseURL: 'http://localhost:8000/api/v1',
  headers: { 'Content-Type': 'application/json' },
})

// Автоматическое добавление токена
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('auth_token')
  if (token) {
    config.headers.Authorization = `Token ${token}`
  }
  return config
})

// Обработка ошибок авторизации
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      localStorage.removeItem('auth_token')
      window.location.href = '/login'
    }
    return Promise.reject(error)
  }
)
```

---

## 9. Темизация и дизайн

### Vuetify тема

```javascript
const vuetify = createVuetify({
  theme: {
    defaultTheme: 'publishingTheme',
    themes: {
      publishingTheme: {
        dark: false,
        colors: {
          primary: '#2D3E50',
          secondary: '#C9A959',
          accent: '#8B4513',
          background: '#F5F2EB',
          surface: '#FFFFFF',
        }
      },
      darkTheme: {
        dark: true,
        colors: {
          primary: '#C9A959',
          secondary: '#2D3E50',
          background: '#1A1A2E',
          surface: '#252540',
        }
      }
    }
  }
})
```

### Особенности дизайна

- **Типографика:** шрифт Cormorant Garamond для заголовков (издательская тематика)
- **Цветовая схема:** тёмно-синий (#2D3E50) и золотой (#C9A959)
- **Переключение тем:** светлая и тёмная тема
- **Адаптивность:** корректное отображение на всех устройствах
- **Анимации:** плавные переходы между страницами

---

## 10. Установка и запуск

### Backend (из laboratory_work_3)

```bash
cd laboratory_work_3/publishing_house
source venv/bin/activate
pip install django-cors-headers
python manage.py runserver
```

### Frontend

```bash
cd laboratory_work_4/publishing_frontend
npm install
npm run dev
```

**URL адреса:**
- Frontend: http://localhost:5173
- Backend API: http://localhost:8000/api/v1
- API Docs: http://localhost:8000/api/docs/

---

## 11. Выводы

В ходе выполнения лабораторной работы были достигнуты следующие результаты:

**Реализованный функционал:**

- ✅ Настройка CORS для взаимодействия frontend ↔ backend
- ✅ Интерфейсы авторизации и регистрации
- ✅ Управление профилем пользователя
- ✅ CRUD интерфейсы для всех сущностей системы
- ✅ Интерфейсы для 6 аналитических отчётов
- ✅ Подключена библиотека Vuetify 3
- ✅ Реализована поддержка светлой и тёмной темы

**Полученные навыки:**

- Работа с Vue 3 Composition API
- Использование Vuetify 3 для создания UI
- Управление состоянием с Pinia
- Маршрутизация с Vue Router
- Интеграция с REST API через Axios
- Реализация аутентификации на клиенте
- Создание адаптивных интерфейсов

