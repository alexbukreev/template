# React + Vite + Tailwind v4 + shadcn/ui — Starter Template

Минимальный шаблон на **React + Vite + TypeScript** с готовой связкой **Tailwind v4** и **shadcn/ui**, автопереключением темы (light/dark/system) и аккуратной структурой каталогов. Подходит как база для быстрых прототипов и небольших приложений.

## Что входит

* ⚡️ Vite + React + TypeScript (строгий `tsconfig`)
* 🎨 Tailwind v4 + кастомные CSS-переменные в OKLCH, готовые light/dark темы
* 🧩 shadcn/ui (конфиг и алиасы настроены, компоненты добавляются командой)
* 🌗 Хук `useAutoThemeClass()` — автоматическое применение темы + реакция на системные изменения
* 🧭 Импорт по алиасу `@` → `src/*`
* 🧹 ESLint (React Hooks + TS rules)
* ⚙️ Vite: `base: '/template/'`, `outDir: 'docs'` — удобно для GitHub Pages

---

## Быстрый старт

Требования: **Node 18+**

```bash
# Установка зависимостей
npm i

# Режим разработки
npm run dev

# Продакшен-сборка (в /docs)
npm run build

# Локальный предпросмотр собранной версии
npm run preview
```

> **Примечание про GitHub Pages:** проект собирается в `docs/` и публикуется с базовым путём `/template/`.
> Если репозиторий называется иначе — измените `base` в `vite.config.ts` (например, на `'/my-app/'` или `'/'`).

---

## Структура проекта

```
template/
├─ public/
├─ scripts/
├─ src/
│  ├─ assets/
│  ├─ components/
│  │  ├─ ui/                 # место для компонентов shadcn/ui
│  │  └─ TextSection.tsx     # пример текстового блока (рыба)
│  ├─ hooks/
│  │  └─ useAutoThemeClass.ts# авто-тема (light/dark/system)
│  ├─ lib/
│  │  └─ utils.ts
│  ├─ App.tsx                # шапка + макет контента
│  ├─ index.css              # Tailwind + темы + токены
│  ├─ main.tsx               # вход, монтирование React
│  └─ vite-env.d.ts
│
├─ components.json           # конфиг shadcn/ui
├─ eslint.config.js          # ESLint настройки
├─ index.html                # корневой HTML (Vite)
├─ package.json
├─ README.md
├─ tailwind.config.js
├─ tsconfig.json
├─ tsconfig.app.json
├─ tsconfig.node.json
└─ vite.config.ts
```

---

## Макет приложения

`src/App.tsx` — минимальный каркас страницы со «шапкой» и местом для контента:

```tsx
import TextSection from './components/TextSection';
import { useAutoThemeClass } from './hooks/useAutoThemeClass';

export default function App() {
  useAutoThemeClass();

  return (
    <>
      <div className="sticky top-0 z-10 w-full py-3 text-center text-3xl font-bold bg-background border-b border-gray-300">
        Project Name
      </div>
      <main className="mx-auto max-w-screen-md px-4 space-y-8">
        <TextSection />
      </main>
    </>
  );
}
```

Замените `TextSection` на свои блоки/страницы.

---

## Стили и тема

Основные моменты в `src/index.css`:

* Подключение Tailwind:

  ```css
  @import "tailwindcss";
  @import "tw-animate-css";
  ```
* Кастом-вариант `dark` для удобной записи:

  ```css
  @custom-variant dark (&:is(.dark *));
  ```
* Две палитры (light и dark) через CSS-переменные OKLCH:
  значения в `:root { ... }` и `.dark { ... }`.
* Базовые слои:

  ```css
  @layer base {
    * { @apply border-border outline-ring/50; }
    body { @apply bg-background text-foreground; }
  }
  ```

### Переключение темы

Хук `useAutoThemeClass()`:

* Читает `localStorage.theme` (`'light' | 'dark' | 'system'`)
* Реагирует на системный `prefers-color-scheme`
* Ставит/снимает класс `dark` на `<html>`

Помощник для ручного переключения (можно вызывать из любого компонента):

```ts
import { setTheme } from '@/hooks/useAutoThemeClass';

// 'light' | 'dark' | 'system'
setTheme('dark');
```

---

## shadcn/ui

Всё уже сконфигурировано (`components.json`, алиасы, Tailwind):

```bash
# пример добавления компонента
npx shadcn@latest add button
```

Компоненты попадут в `src/components/ui/`.
Параметры (стиль, пути, алиасы) берутся из `components.json`.

---

## Импорт по алиасам

Алиасы настроены в `vite.config.ts` и `tsconfig.*.json`:

* `@` → `src/*`
* дополнительные алиасы для shadcn/ui (см. `components.json`)

Пример:

```ts
import { setTheme } from '@/hooks/useAutoThemeClass';
```

---

## Линтинг

ESLint включён для TypeScript + React Hooks:

* Конфиг: `eslint.config.js`
* Запуск (пример):

  ```bash
  npx eslint "src/**/*.{ts,tsx}"
  ```

---

## Деплой на GitHub Pages

1. В `vite.config.ts` проверьте:

   * `base: '/template/'` — замените на `'/<имя-репозитория>/'`
     или `'/'`, если деплой не на Pages.
   * `build.outDir = 'docs'`.
2. `npm run build` — артефакты окажутся в `docs/`.
3. В настройках репозитория включите Pages → Source: **/docs**.

---

## Кастомизация

* **Порт dev-сервера:** `vite.config.ts → server.port` (по умолчанию `5175`)
* **Палитра/радиусы/токены:** правьте переменные в `:root` и `.dark` в `index.css`
* **Шапка проекта:** меняйте заголовок в `App.tsx`
* **Компоненты UI:** добавляйте через `shadcn` CLI, храните в `src/components/ui`

---

## FAQ

**Почему сборка идёт в `docs/`?**
Удобно для GitHub Pages. Если не нужно — смените `outDir` и `base`.

**Можно убрать тёмную тему?**
Да. Удалите хук `useAutoThemeClass()`, `@custom-variant dark`, и блок `.dark { ... }` из `index.css`.

**Хочу использовать абсолютные импорты.**
Уже настроено: используйте `@/...`.

---

## Лицензия

Выберите и добавьте свою (например, MIT). В шаблоне лицензия не задана.
