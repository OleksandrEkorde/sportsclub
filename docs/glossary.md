# Словник реалізації

Канонічним джерелом назв класів і методів є `class.mmd`. Ідентифікатори в
діаграмах та CRC-картках залишаються англійськими; пояснення й обов'язки
наведені українською.

| Канонічна назва | Призначення |
|---|---|
| `ClubParticipant` | Абстрактна спільна основа учасників клубу; визначає конструктор базових даних |
| `ClubMember` | Купує абонемент, бронює заняття та подає запит на заморожування |
| `Instructor` | Формує розклад групових занять |
| `Administrator` | Переглядає заповненість залів |
| `IClassSchedule` | Контракт перегляду й оновлення розкладу |
| `Membership` | Керує строком дії та станом абонемента; отримує `AccessControlSystem` через конструктор |
| `Visit` | Фіксує участь або бронювання члена клубу на заняття |
| `AccessControlSystem` | Перевіряє право доступу за членством |
| `TrainingSession` | Описує групове заняття та доступні місця |
| `Hall` | Описує зал, його місткість і робочий час |

## Канонічні операції

| Клас | Методи |
|---|---|
| `ClubParticipant` | `ClubParticipant(participantId, name)`, `getRole()` |
| `ClubMember` | `ClubMember(name, participantId)`, `purchaseMembership(membership)`, `freezeMembership(membership)`, `bookSession(session)` |
| `Instructor` | `Instructor(name, participantId)`, `createSession(session)` |
| `Administrator` | `Administrator(name, participantId)`, `viewHallOccupancy(hall)` |
| `IClassSchedule` | `viewSchedule()`, `updateSchedule(session)` |
| `Membership` | `Membership(membershipId, startDate, endDate, accessControlSystem)`, `freeze(startDate, endDate)`, `reactivate()`, `validate()` |
| `Visit` | Конструктор `Visit(visitId, visitDate, session, member)` |
| `AccessControlSystem` | `AccessControlSystem()`, `checkAccess(member, membership)`, `grantAccess(visit)` |
| `TrainingSession` | `TrainingSession(sessionId, startTime, endTime, capacity, hall)`, `getAvailableSeats()`, `validate()` |
| `Hall` | `Hall(hallId, name, maxCapacity)`, `getOccupancy()`, `getAvailableSeats()`, `isWithinOpeningHours(startTime, endTime)` |

`Schedule` і `OccupancyReport` — типи значення, що повертаються методами, а не
додаткові сутності предметної області чи учасники діаграм.
