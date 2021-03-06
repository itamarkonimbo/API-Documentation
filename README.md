# Konimbo API Docs

### תוכן עניינים
1. [הקדמה](#user-content-הקדמה)
2. [הגרסא הנוכחית](#user-content-הגרסא-הנוכחית)
3. [סכמה](#user-content-סכמה)
4. [אוטנטיקציה](#user-content-אוטנטיקציה)
5. [עימוד](#user-content-עימוד)
6. [הגבלת קריאות](#user-content-הגבלת-קריאות)
7. [נקודות קצה](#user-content-נקודות-קצה)

### הקדמה

המדריך הזה מיועד למפתחים אשר מעוניינים להתממשק עם המערכת של קונימבו.

לבעיות או שאלות נוספות ניתן לפנות ל[תמיכה](http://konimbo.co.il/pages/4659-%D7%A6%D7%95%D7%A8-%D7%A7%D7%A9%D7%A8)

### הגרסא הנוכחית
הגרסא הנוכחית באוויר היא v1.

ולכן כל הקריאות לשרת הAPI יתבצעו בEndPoint הבא:
```
https://api.konimbo.co.il/v1
```

### סכמה
* כל הקריאות יתבצעו בפרוטוקול https

* מידע אשר נשלח בגוף הקריאה (Request Body) יישלח בתצורת JSON

* התגובה מהשרת תהיה כברירת מחדל בפורמט JSON, כדי לשנות את הפורמט לXML יש ליצור קשר עם התמיכה

* כל הזמנים באים לידי ביטוי בפורמט [ISO-8601](https://www.w3.org/TR/NOTE-datetime)
```
YYYY-MM-DDTHH:MM:SSZ
```

### אוטנטיקציה
לכל קריאה לשרתי הAPI
חובה לתת פרמטר `token`.

את פרמטר הטוקן יש לקבל מבעל החנות שאליה תרצו להתממשק

ללא טוקן פעיל, יוחזר מהשרת `Http Response 401 Unauthorized`

### עימוד
כברירת מחדל, לקריאות אשר מוחזרים להם כמות נתונים מעל 20 רשומות,
הנתונים יחזרו בקבוצות של 20 לכל דף.

ניתן לקבוע כמות אחרת לעימוד, כדי לעשות זאת, צור קשר עם התמיכה.

Header| הסבר
---|---
X-Pagination-Per-Page | כמות התוצאות לכל דף
X-Pagination-Total | כמות התוצאות סך הכל
X-Pagination-Links | לינקים לדף הראשון, האחרון, הקודם והבא. בפורמט [Header Link](https://www.w3.org/wiki/LinkHeader)

### הגבלת קריאות
כברירת מחדל, הגבלת הקריאות מתבצעת פר טוקן ל100 קריאות ב10 דקות.

כלומר, בכל 10 דקות תתאפס הספירה של כמות הקריאות.

Header| הסבר
---|---
X-Rate-Limit-Current | מספר הקריאה בספירה הנוכחית
X-Rate-Limit-Maximum | כמות הקריאות שתגרום לחסימת המשתמש
X-Rate-Limit-Reset   | השעה שבה תתאפס הספירה ויהיה ניתן לבצע קריאות מחדש

בעת חסימה, יוחזר מהשרת `Http Response 401 Unauthorized`

### נקודות קצה
1. [מוצרים](https://github.com/konimboltd/api-documentation/blob/master/v1/items.md)
2. [הזמנות](https://github.com/konimboltd/api-documentation/blob/master/v1/orders.md)
3. [קבצים](https://github.com/konimboltd/api-documentation/blob/master/v1/amazons.md)
4. [עגלות](https://github.com/konimboltd/api-documentation/blob/master/v1/carts.md)


