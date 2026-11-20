# Модель даних UML-діаграм

## Сутності та атрибути

| Сутність | Основні поля і методи | Відповідальність |
|---|---|---|
| `ClubParticipant` | `constructor(participantId, name)`, `participantId: String`, `name: String`, `abstract getRole(): String` | Абстрактна спільна характеристика учасників клубу |
| `ClubMember` | `constructor(name, participantId)`, `purchaseMembership(membership: Membership): void`, `freezeMembership(membership: Membership): void`, `bookSession(session: TrainingSession): Booking` | Купівля абонемента, бронювання і заморожування |
| `Instructor` | `constructor(name, participantId)`, `createSession(session: TrainingSession): void` | Створення і ведення записів розкладу |
| `Administrator` | `constructor(name, participantId)`, `viewHallOccupancy(hall: Hall): OccupancyReport` | Контроль заповненості залів |
| `IClassSchedule` | `viewSchedule(): Schedule`, `updateSchedule(session: TrainingSession): void` | Контракт операцій із розкладом |
| `Membership` | `constructor(membershipId, startDate, endDate, accessControlSystem)`, `membershipId`, `startDate`, `endDate`, `state`, `accessControlSystem: AccessControlSystem`, `freeze(startDate, endDate): void`, `reactivate(): void`, `final validate(): boolean` | Керування життєвим циклом абонемента |
| `Visit` | `visitId`, `visitDate`, `session: TrainingSession`, `member: ClubMember` | Фіксація участі члена клубу в занятті |
| `AccessControlSystem` | `constructor()`, `checkAccess(member: ClubMember, membership: Membership): boolean`, `grantAccess(visit: Visit): boolean` | Перевірка права відвідування |
| `TrainingSession` | `constructor(sessionId, startTime, endTime, capacity, hall)`, `sessionId`, `startTime`, `endTime`, `capacity`, `hall: Hall`, `getAvailableSeats(): int`, `validate(): boolean` | Групове заняття, перевірка даних і вільні місця |
| `Hall` | `constructor(hallId, name, maxCapacity)`, `hallId`, `name`, `maxCapacity`, `getOccupancy(): int`, `getAvailableSeats(): int`, `isWithinOpeningHours(startTime, endTime): boolean` | Поточна і максимальна заповненість та робочий час |

`Booking` і `OccupancyReport` у методах не є обов'язковими ключовими класами предметної області і мають бути значеннями результату або розширеннями лише за потреби в генерованому плані реалізації.

## Зв'язки

- `ClubParticipant` є абстрактним базовим класом для `ClubMember`, `Instructor` і `Administrator`.
- `ClubMember`, `Instructor` і `Administrator` реалізують `IClassSchedule`.
- `Membership` залежить від `AccessControlSystem` для перевірки права доступу.
- `Visit` має композиційний або агрегаційний зв'язок із `TrainingSession` і залучає `ClubMember`.
- `TrainingSession` асоційований із `Hall`.
- `ClubMember` може створювати багато екземплярів `Visit` через бронювання і відвідування.
- Кожен екземпляр ролі створюється через конструктор похідного класу.

## Правила валідації

- Кількість учасників групового заняття не перевищує обмеження залу і ліміт заняття.
- Переходи стану абонемента не дозволені з `Cancelled` або `Expired`.
- Період заморожування має початок і кінець, причому кінець не передує початку.
- Повторне заморожування вже замороженого абонемента є ідемпотентною операцією або помилкою без зміни даних.
- Спроба забронювати останнє місце одночасно лише один раз може створити успішне бронювання.
- Створення заняття поза робочим часом залу відхиляється.

## Стани абонемента

```text
Inactive -> Active -> Frozen -> Active
Active -> Expired
Active -> Cancelled
Inactive -> Cancelled
```

Кінцевий стан діаграми станів визначає термінальний стан сценарію моделювання, але є окремим від фінального стану предметної сутності.
