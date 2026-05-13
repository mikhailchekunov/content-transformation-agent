# Examples Library: Clickbait Message Transformer

This file stores real-world examples collected from user feedback.
Agents may reference this file to calibrate tone, style, and quality.

---

## How This File Works

Examples are added automatically after user feedback:
- User score **5** → added to Good Examples
- User score **1 or 2** → added to Bad Examples
- User score **3 or 4** → not stored

Each example contains:
- Date and time added
- Original news text
- Generated message
- Validator scores (Vibe-Check / Utility / Hook / Accuracy)
- User score

---

## Good Examples (User Score: 5)

> Examples here represent the gold standard — messages that felt human,
> delivered clear value, and made the reader want to engage immediately.

<!-- GOOD_EXAMPLES_START -->

### Example 1

**Added:** 2026-05-13 14:30
**User Score:** 5

**Original News:**
25 июня в московской «Live Арене» пройдёт Young Con — молодёжный фестиваль «Яндекса» о технологиях, искусственном интеллекте и старте карьеры в IT.
Мероприятие проводят уже третий год подряд. В этом году участие примут 10 тыс. старшеклассников, студентов и начинающих специалистов.
В программе фестиваля заявлены лекции о технологиях и карьерных траекториях в IT-индустрии.
С докладами выступят HR-директор, руководитель образовательных проектов и технологий для общества Даша Золотухина, CTO поисковых сервисов и ИИ Алексей Гусаков, а также CEO Алисы и умных устройств Валерий Стромов.
Отдельный блок будет посвящён тому, как начать карьеру. Для участников будут доступны консультации и возможность пообщаться с представителями команды «Яндекса».
Кроме того, свои силы можно будет попробовать в пробных собеседованиях. По итогам отбора участникам предложат программы стажировок с упрощённой процедурой зачисления.
Помимо этого на фестивале организуют стенд с аппаратными разработками «Яндекса», включая умные колонки, автономный транспорт и роботов-доставщиков.
Также в рамках Young Con пройдут финалы Yandex ML Challenge и «Битвы вузов». Призовые фонды каждого турнира составляют 1 млн рублей.
Полная программа фестиваля Young Con и форма регистрации доступны на официальном сайте.

**Generated Message:**
На Young Con 25 июня можно прямо на фестивале пройти пробное собеседование в «Яндекс» и попасть на стажировку через упрощённый отбор. Разберу, к чему конкретно готовиться, за пять минут.

**Validator Scores:**
- Vibe-Check: 3
- Utility: 3
- Hook: 3
- Accuracy: 3

**Notes:**

<!-- GOOD_EXAMPLES_END -->

---

## Bad Examples (User Score: 1–2)

> Examples here failed — either sounded like a bot, lacked value,
> had a weak CTA, or distorted the original facts.
> Use these to understand what patterns to avoid.

<!-- BAD_EXAMPLES_START -->

*(No examples yet. They will appear here after users rate messages 1–2/5.)*

<!-- BAD_EXAMPLES_END -->

---

## Example Format Reference

When adding a new example, use this exact format:

~~~
### Example [N]

**Added:** YYYY-MM-DD HH:MM
**User Score:** [1–5]

**Original News:**
[paste original news text here]

**Generated Message:**
[paste final output message here]

**Validator Scores:**
- Vibe-Check: [1–3]
- Utility: [1–3]
- Hook: [1–3]
- Accuracy: [1–3]

**Notes:** *(optional — why this example is good or bad)*
~~~