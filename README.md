# FitZone

Вебзастосунок онлайн-бронювання занять фітнес-студії (лабораторні роботи № 1–5).

| Частина | Технології | Де |
|---|---|---|
| Клієнт | HTML, CSS (лаб. № 3), JavaScript + Fetch API (лаб. № 4–5) | `public/` |
| Сервер | Node.js, Express 5, REST API (JSON) | `server/` |
| База даних | SQLite (вбудований модуль `node:sqlite`) | `data/fitzone.db` (створюється автоматично) |

## Запуск локально

Потрібен Node.js ≥ 22.13.

```bash
npm install
npm start            # http://localhost:3000
npm test             # автоматичні тести API
```

У розробці `JWT_SECRET` генерується випадково при кожному старті (сесії скидаються після перезапуску).
Для production задайте змінні з `.env.example`. Тестова картка для оплати: `4111 1111 1111 1111`, будь-який майбутній термін, CVV із 3 цифр.

## REST API

| Метод і шлях | Призначення | Доступ |
|---|---|---|
| `GET /api/health` | перевірка стану сервера | усі |
| `POST /api/auth/register` | реєстрація | усі |
| `POST /api/auth/login`, `POST /api/auth/logout` | вхід / вихід | усі |
| `GET /api/auth/me` | поточний користувач (`user: null` для гостя) | усі |
| `PATCH /api/me`, `DELETE /api/me` | оновлення профілю й нагадувань / видалення акаунту | авторизовані |
| `GET /api/classes?kind=&trainer=&period=` | розклад із фільтрами та вільними місцями | усі |
| `POST /api/bookings` | резервування місця на 15 хв | авторизовані |
| `POST /api/bookings/:id/pay` | оплата (перевірка картки, дані не зберігаються) | авторизовані |
| `GET /api/bookings`, `GET /api/bookings/:id` | мої бронювання / одне бронювання | авторизовані |
| `DELETE /api/bookings/:id` | скасування (не пізніше ніж за 3 год) | авторизовані |
| `POST /api/reviews`, `GET /api/reviews` | залишити відгук / список відгуків | авторизовані / усі |

## Розгортання

Сервер потребує Node.js, тому GitHub Pages не підходить. Див. `render.yaml` (Render.com) —
Web Service, `npm ci` / `npm start`, змінні `NODE_ENV=production`, `JWT_SECRET`, `DB_PATH`.
Без постійного диска база SQLite скидається при перезапуску сервісу; розклад занять створюється автоматично.
