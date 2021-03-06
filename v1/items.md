# Konimbo API Docs
## מוצרים
### תוכן עניינים
0. [חזור לדף הראשי](https://github.com/konimboltd/api-documentation)
1. [הקדמה](#user-content-הקדמה)
2. [EndPoints](#user-content-endpoints)
3. [פירוט השדות](#user-content-פירוט-השדות)
4. [מוצר](#user-content-מוצר)
5. [מוצרים](#user-content-מוצרים)
6. [יצירת מוצר](#user-content-יצירת-מוצר)
7. [עריכת מוצר](#user-content-עריכת-מוצר)
8. [מחיקת מוצר](#user-content-מחיקת-מוצר)

### הקדמה
לכל חנות יש מוצרים,
ולכל מוצר יש קטגוריה שאליה הוא משויך.

ניתן למצוא מוצר לפי
* id
* code
* second_code

כברירת מחדל, אפשר למצוא מוצר לפי id, כדי לשנות הגדרה זו, נא ליצור קשר עם התמיכה.
### EndPoints
* [GET /{apiVersion}/items](#user-content-מוצר)
* [POST /{apiVersion}/items](#user-content-יצירת-מוצר)
* [GET /{apiVersion}/items/{id|code|second_code}](#user-content-מוצרים)
* [PUT /{apiVersion}/items/{id|code|second_code}](#user-content-עריכת-מוצר)
* [DELETE /{apiVersion}/items/{id|code|second_code}](#user-content-מחיקת-מוצר)

### פירוט השדות
#### שדות פשוטים:
שם השדה | סכמה | ניתן לעדכון | הסבר | דוגמא
:--|:---|---:|---:|---:
id                     | Integer         | לא | מספר המוצר במערכת קונימבו | `1203546`
title                  | String          | כן | כותרת המוצר | `טלפון סלולארי`
store_category_title   | String          | כן | הקטגוריה שאליה המוצר שייך | `טלפונים`
price                  | Numeric         | כן | מחיר המוצר | `55.50`, `999`
origin_price           | Numeric         | כן | מחיר מקורי (לפני מבצע) | `1499`, `1999.99`
code                   | String          | כן | מק"ט המוצר | `SKU#123`, `49841687468`
second_code            | String          | כן | מקט משני (לשימוש פנימי) | `6418351816`
desc                   | String          | כן | תיאור קצר על המוצר | `טלפון מתקדם מיצרן בינלאומי, מספר אחד בתחום הטלפונים`
visible                | Boolean         | כן | האם המוצר מופיע בחנות או מוסתר | `true`, `false`
model_title            | String          | כן | שם הספק | `MyProvider`
brand                  | String          | כן | שם היצרן | `Manufacturer`, `יצרן`
created_at             | Time (ISO-8601) | לא | זמן יצירת המוצר | `2016-01-01T09:00:00Z`
updated_at             | Time (ISO-8601) | לא | זמן העדכון האחרון של המוצר | `2017-01-01T09:00:00Z`
position               | Integer         | כן | מיקום המוצר | `123`
warranty               | String          | כן | אחריות | `5 שנים`
#### שדות מיוחדים:
השדות המיוחדים מיוצגים ע"י אובייקטים לפי הפירוט הבא

##### תמונות:
שדה זה ניתן לעדכון.

לכל מוצר יש תמונות, בכל קריאה ניתן להעלות עד 12 תמונות
יש לשים לב שהתמונה מורשת לקריאה, ניתן לבדוק זאת ע"י גישה לתמונה דרך הדפדפן.

בנוסף, תמונות פועלות בצורה שונה מכל שאר השדות: עבור כל קריאה לעדכון מוצר שמכילה תמונות, התמונות שישלחו הם התמונות שהמוצר יכיל
כל תמונה אחרת שלא מופיעה באותה בקשה ספציפית - ימחקו. 

ז"א בכל התייחסות לתמונות של מוצר - יש לשלוח את "תמונת המצב" שנרצה עבור המוצר מבחינת התמונות שלו.
במידה ולא נרצה להתייחס לתמונות, לא נשלח את השדה images כלל.

שם השדה | סכמה | חובה | הסבר
:---|:---|---:|---:
url | String | לא | הUrl של התמונה שברצונך להעלות<br>פורמטים מורשים: doc, docx ,xml ,jpeg ,gif ,png ,bmp ,pdf
alt | String | לא | הטקסט החלופי של התמונה
position | String | לא | מיקום התמונה ביחס לשאר התמונות

דוגמאות:

* יצירת תמונה חדשה
```JSON
"images": [{
    "url": "http://domain.com/images/myImage.jpg",
    "alt": "תיאור התמונה",
    "position": "1"
}]
```
* עריכת תמונה קיימת. שים לב: חיפוש התמונה מתבצע לפי כתובת התמונה
```JSON
"images": [{
    "url": "http://domain.com/images/myImage.jpg",
    "alt": "תיאור התמונה",
    "position": "1"
}]
```
* מחיקת תמונה

מחיקת תמונה מתבצעת ע"י אי שליחתה בעת עדכון מוצר.
לדוגמא אם למוצר יש 2 תמונות: תמונה א' ותמונה ב', אם נרצה למחוק את תמונה ב', נשלח בקשת עדכון למוצר שתכיל רק את תמונה א.


* מחיקת כל התמונות
```JSON
"images": [{
    "url": "",
    "alt": "",
    "position": "",
    "delete": "true"
}]
```
* מחיקת כל התמונות ויצירת חדשות

מתבצע באופן אוטומטי - תמיד יש לשלוח את כל התמונות שנרצה שישוייכו למוצר - גם אם הן קיימות.
במידה ונרצה למחוק קיימות, לא נשלח אותן בפעולת העדכון

##### מוצרים נלווים:
שדה זה ניתן לעדכון.
לכל מוצר ניתן לשייך מוצרים נוספים מהחנות - מוצרים נלווים.
עבור כל מוצר נלווה יש לשלוח את הנתונים הבאים:
* ID של המוצר במערכת קונימבו
* מיקום המוצר

שם השדה | סכמה | חובה | הסבר
:---|:---|---:|---:
friend_item_id | String | כן | מזהה מוצר 
position | String | לא | מיקום המוצר

דוגמאות:

* יצירת שורה חדשה

```JSON
"related_items": [{
			"friend_item_id": "1358140",
			"position": "1"
		},{
			"friend_item_id": "1358141",
			"position": "2"
		}]
```

* עריכת שורה קיימת

המערכת תחפש את המוצר הנלווה על פי המזהה.

```JSON
"related_items": [{
			"friend_item_id": "1358140",
			"position": "3"
		}]
```

* מחיקת מוצר נלווה קיים

מחיקת מוצר נלווה קיים תתבצע ע"י אי שליחתו.
תמיד נשלח את "תמונת המצב" של המוצרים הנלווים של המוצר המדובר, וכל שאר הנלווים שלא נשלחו - ימחקו. (תצורת דריסה)

* מחיקת כל המוצרים הנלווים

```JSON
"related_items": [{
			"friend_item_id": null,
			"position": null,
            "delete": "true"
		}]
```

##### מפרט טכני מובנה:
שדה זה ניתן לעדכון.

לכל מוצר יש שורות מפרט מובנה, כל שורת מפרט מכילה בתוכה
* קבוצה
* שם
* ערך

שם השדה | סכמה | חובה | הסבר
:---|:---|---:|---:
group | String | כן | שם הקבוצה
name | String | כן | שם המאפיין
value | String | כן | ערך המאפיין
position | Integer | כן | מיקום השורה הנוכחית ביחס לכלל השורות במפרט
delete | Boolan | כן | כדי למחוק את כל המפרט, יש לשלוח רק את שדה הdelete, כדי למחוק שורה ספציפית יש לספק גם את שדה ה id

דוגמאות:

* יצירת שורה חדשה
```JSON
"spec": [{
    "group": "Physical",
    "name": "Size",
    "value": "20 x 25 cm",
    "position": "9"
}]
```
* עריכת שורה קיימת

המערכת תחפש את שורת המפרט הספציפית לעדכון עפ"י 2 השדות הבאים:

* group

* name

ולכן לא ניתן לערוך את 2 שדות אלה (אלא נצטרך ליצור שורה חדשה)

```JSON
"spec": [{
    "group": "Physical",
    "name": "Weight",
    "value": "100g",
    "position": "10",
}]
```
* מחיקת שורה קיימת

מחיקת שורת מפרט קיימת מתבצעת ע"י אי שליחתה בעת עדכון מוצר.
לדוגמא: אם למוצר יש שורת מפרט א' ושורת מפרט ב' ונרצה למחוק את שורה ב', נשלח עדכון עבור המוצר שיכיל רק את שורת המפרט א'

* מחיקת כל המפרט

```JSON
"spec": [{
    "group": null,
    "name": null,
    "value": null,
    "position": null,
    "delete": "true"
}]
```

##### סינונים:
שדה זה ניתן לעדכון.

דרך שדה זה ניתן לשייך מוצר לסינונים.
במידה והסינון המבוקש לא קיים, הוא יווצר אוטמטית בחנות 

* כותרת קבוצה
* מיקום קבוצה
* ערך קבוצה
* מיקום ערך קבוצה

שם השדה | סכמה | חובה | הסבר
:---|:---|---:|---:
group_title | String | כן | שם הקבוצה
group_value_title | String | כן | שם ערך הקבוצה
group_position | String | כן | מיקום הקבוצה
group_value_position | Integer | כן | מיקום ערך הקבוצה
broad | Boolan | לא | במידה ורוצים ליצור סינון רוחבי יש לשלוח את הערך   true 
remove_from_item | Boolan | לא | כדי למחוק שיוך של סינון למוצר יש לשלוח שדה זה עם הערך true 

דוגמאות:

* שיוך חדש בין מוצר לסינון

במידה וישלח סינון או ערך סינון אשר לא קיים במערכת, הוא יווצר אוטומטית
והמוצר ישוייך אליו.

חיפוש סינון וערך סינון (בכדי לדעת אם ליצור אותו או לא) מתבצע לפי השדות הבאים:

* group_title
* group_value_title

```JSON
"filters": [{
    "group_title": "סוג אביזר",
    "group_value_title": "לחצנים",
    "group_position": "1",
    "group_value_position": "1"
},
{
    "group_title": "סוג אביזר",
    "group_value_title": "ברגים",
    "group_position": "1",
    "group_value_position": "2"
}]
```
* עריכת שיוך קיים בין מוצר לסינון

ניתן רק להסיר שיוך של מוצר לסינון וערך סינון מסויים, לא ניתן לערוך את הסינון עצמו

* מחיקת שיוך קיים בין מוצר לסינון וערך סינון קיימים

```JSON
"filters": [{
    "group_title": "סוג אביזר",
    "group_value_title": "ברגים",
    "group_position": "1",
    "group_value_position": "2",
    "remove_from_item": "true"
}]
```
##### תגים:
שדה זה ניתן לעדכון.

דרך שדה זה ניתן לשייך את המוצר לתגים, וגם ליצור תגים חדשים במידה ולא קיימים


שם השדה | סכמה | חובה | הסבר
:---|:---|---:|---:
title | String | כן | שם התג
group_name | String | לא | שם ערך הקבוצה
seo_description | Integer | לא | מיקום ערך הקבוצה
seo_keywords | Boolan | לא |    true 
seo_title | Boolan | לא |    true 
visable | Boolan | לא |    true 
slug | Boolan | לא |    true 
content | Boolan | לא |    true 
use_name | Boolan | לא | תג רגיל או קטוגריה משנית   true 
remove_from_item | Boolan | לא | כדי למחוק שיוך של סינון למוצר יש לשלוח שדה זה עם הערך true 

דוגמאות:

* שיוך חדש בין מוצר לתג

במידה וישלח תג אשר לא קיים במערכת, הוא יווצר אוטומטית
והמוצר ישוייך אליו.

חיפוש תג (בכדי לדעת אם ליצור אותו או לא) מתבצע לפי השדה הבא:

* title

```JSON
"related_tags": [
			{"title": "miritag"}, {"title": "israeltag"}
		]
```
* עריכת שיוך קיים בין מוצר לתג

ניתן רק להסיר שיוך של מוצר לתג, לא ניתן לערוך את התג עצמו

* מחיקת שיוך קיים בין מוצר לתג קיים

מחיקה של תג מסויים מתבצעת ע"י אי שליחתו בבקשה.
ז"א - התגים שישלחו בבקשה הם התגים שישוייכו למוצר, וכל שאר התגים ששויכו בעבר ימחקו

* מחיקת כל תגים עבור מוצר מסויים
```JSON
"related_tags": [
			{"remove_from_item": "true"}
		]
```

##### קטגוריה משנית:
שדה זה ניתן לעדכון.

דרך שדה זה ניתן לשייך מוצר לקטגוריה משנית אחת או יותר

שם השדה | סכמה | חובה | הסבר
:---|:---|---:|---:
title_he | String | כן | שם הקטגוריה

דוגמאות:

* שיוך חדש בין מוצר לקטגוריה משנית

במידה ותשלח קטגוריה משנית אשר לא קיימת במערכת, היא תיווצר אוטומטית
והמוצר ישוייך אליה.

חיפוש קטגוריה משנית (בכדי לדעת אם ליצור אותה או לא) מתבצע לפי השדה הבא:

* title_he

```JSON
"secondary_category_titles": [
			{"title_he": "itamarbla"}, {"title_he": "ziv_is_in_the_"}
		]
```
* עריכת שיוך קיים בין מוצר לקטגוריה משנית

ניתן רק להסיר שיוך של מוצר לקטגוריה משנית, לא ניתן לערוך את הקטגוריה המשנית עצמה

* מחיקת שיוך קיים בין מוצר לקטגוריה משנית 

מחיקה של קטגוריה משנית מסויימת מתבצעת ע"י אי שליחתה בבקשה.
ז"א - הקטגוריות המשניות שישלחו בבקשה הן הקטגוריות המשניות שישוייכו למוצר, וכל שאר הקטגוריות המשניות ששויכו בעבר ימחקו

* מחיקת כל הקטגוריות המשניות עבור מוצר מסויים
```JSON
"secondary_category_titles": [
			{"remove_from_item": "true"}
		]
```


##### מחירי מועדון:
שדה זה ניתן לעדכון.

field_XXX - XXX מייצג את הid של מועדון הלקוחות
```
"discount_prices": {
  "field_1211": "60.0",
  "field_1212": "65",
  "field_1213": "69.99"
}
```
##### מלאי:
שדה זה ניתן לעדכון.

שם השדה | סכמה | חובה | הסבר
:---|:---|---:|----:
id | Integer | לא | מספר השדרוג מלאי במערכת קונימבו, יש לספק רק אם ברצונך לערוך את המקט
code | String | כן | המק"ט שלפיו יש לעדכן את המלאי
price | Numeric | לא | תוספת מחיר לוריאציה הזו
free | Numeric | לא | כמות מוצרים במלאי

דוגמא:
```JSON
"inventory": [{
  "id": "5785468",
  "code": "PHONE#GOLD",
  "price": "100",
  "free": "99"
}, {
  "id": "5785469",
  "code": "PHONE#SILVER",
  "price": "0",
  "free": "102"
}]
```

### מוצר
#### תיאור
בעזרת השירות הזה, ניתן להחזיר מידע על מוצר ספציפי

ניתן לבקש מהשרת רק את השדות שתרצו

#### EndPoint
```
GET /{apiVersion}/items/{id|code|second_code}?token={yourToken}
Host: api.konimbo.co.il
```

#### Parameters
##### בקשת שדות מסויימים
כדי שהשרת יידע איזה שדות להחזיר יש לציין בפרמטר attributes את השדות המבוקשים מופרדים בפסיקים.

[לרשימת השדות](#user-content-פירוט-השדות)

לדוגמא: `attributes=id,price,code`

במידה ולא נשלח פרמטר זה, יוחזרו כל השדות המורשים למשתמש זה.

#### Responses
* 200 - The requested item
* 401 - Unauthorized
* 404 - No results found
* 500 - Internal error

#### דוגמאות
הקריאה הבאה תחזיר את כל השדות המורשים למוצר עם הid 1234567
```
GET /v1/items/1234567?token=e012a2944be024a85062c2bbd27b9f61284bf2439fd87992069b84b6a4ef93ca HTTP/1.1
Host: api.konimbo.co.il
```

הקריאה הבאה תחזיר את השדות title,store_category_title של המוצר עם הid1234567
```
GET /v1/items/1234567?token=e012a2944be024a85062c2bbd27b9f61284bf2439fd87992069b84b6a4ef93ca&attributes=title,store_category_title HTTP/1.1
Host: api.konimbo.co.il
```

**ניתן לחפש לפי מק"ט או לפי מקט משני, כדי לעשות זאת יש ליצור קשר עם התמיכה**

### מוצרים
#### תיאור
בעזרת השירות הזה, ניתן להחזיר מידע על מספר מוצרים.

ניתן לסנן ולמיין את התוצאות, ובנוסף ניתן לבקש מהשרת שיחזיר רק את השדות שתרצו.

#### EndPoint
```
GET /{apiVersion}/items?token={yourToken}
Host: api.konimbo.co.il
```

#### Parameters
את כל הפרמטרים ניתן לשלב יחד.
##### סינון

פרמטר | הסבר | דוגמא
:---|---:|:---
created_at_min | מחזיר את כל המוצרים שנוצרו אחרי הזמן הנתון | `created_at_min=2017-01-01T09:00:00Z`
created_at_max | מחזיר את כל המוצרים שנוצרו לפני הזמן הנתון | `created_at_max=2016-01-01T09:00:00Z`
updated_at_min | מחזיר את כל המוצרים שנערכו אחרי הזמן הנתון | `updated_at_min=2017-01-01T09:00:00Z`
updated_at_max | מחזיר את כל המוצרים שנערכו לפני הזמן הנתון | `updated_at_max=2016-01-01T09:00:00Z`
min_price | מחזיר את כל המוצרים עם מחיר מעל המחיר שניתן | `min_price=100`
max_price | מחזיר את כל המוצרים עם מחיר מתחת למספר שניתן | `max_price=1000`
group_value_id | מחזיר את כל המוצרים עם הid של הסינון שניתן | `group_value_id=56482`
visible | מחזיר את כל המוצרים המופיעים בחנות או הלא מופיעים בחנות | `visible=true`, `visible=false`
tag_id | מחזיר את כל המוצרים השייכים לתג id שניתן | `tag_id=54872`
store_category_id | מחזיר את כל המוצרים השייכים לקטגוריה עם הid שניתן | `store_category_id=65587`

##### מיון

פרמטר | הסבר | דוגמא
:---|---:|:---
sort_by | מיון סדר התוצאות לפי פרמטר מסוים | `sort_by=price`, `sort_by=updated_at`
sort_order | מיון סדר התוצאות בסדר עולה או יורד, פרמטר זה מתפקד רק ביחד עם `sort_by`| `sort_order=desc`, `sort_order=asce`

##### בקשת שדות מסויימים
כדי שהשרת יידע איזה שדות להחזיר יש לציין בפרמטר `attributes` את השדות המבוקשים מופרדים בפסיקים. 

[לרשימת השדות](#user-content-פירוט-השדות)

לדוגמא: `attributes=id,price,code`

במידה ולא נשלח פרמטר זה, יוחזרו כל השדות המורשים למשתמש זה.
#### Responses

* 200 - Array of items
* 400 - Bad request
* 401 - Unauthorized
* 404 - No results found
* 500 - Internal error

#### דוגמאות
הקריאה הבאה מבקשת את השדות id, title, code של המוצרים המופיעים שהמחיר שלהם בין 100 ל1000 והם משויכים לתג 34567, כאשר הם ממוינים לפי תאריך העדכון האחרון בסדר יורד.
```
GET /v1/items?token=e012a2944be024a85062c2bbd27b9f61284bf2439fd87992069b84b6a4ef93ca&attributes=id,title,code&sort_by=updated_at&sort_order=desc&tag_id=34567&min_price=100&max_price=1000&visible=true HTTP/1.1
Host: api.konimbo.co.il
```

הקריאה הבאה מבקשת את כל השדות המורשים למוצרים שנוצרו אחרי 01.01.17 בשעה 00:00

```
GET /v1/items?token=e012a2944be024a85062c2bbd27b9f61284bf2439fd87992069b84b6a4ef93ca&created_at_min=2017-01-01T00:00:00Z HTTP/1.1
Host: api.konimbo.co.il
```

### יצירת מוצר
#### תיאור
בעזרת שירות זה, ניתן ליצור מוצר חדש.
#### EndPoint
```
POST /{apiVersion}/items HTTP/1.1
Host: api.konimbo.co.il
Content-Type: application/json

{
  "token": "{yourToken}",
  "item": {
    "attribute1": "value",
    "attribute2": "value"
  }
}
```
#### שדות שניתן לעדכן למוצר
ראה [פירוט השדות](#user-content-פירוט-השדות)

שדות חובה:
* title
* store_category_title

#### Responses
* 200 - The created item
* 400 - Bad Request
* 401 - Unauthorized
* 422 - Unprocessable Entity
* 500 - Internal error

#### דוגמאות

```
POST /v1/items HTTP/1.1
Host: api.konimbo.co.il
Content-Type: application/json

{
  "token": "e012a2944be024a85062c2bbd27b9f61284bf2439fd87992069b84b6a4ef93ca",
  "item": {
    "title": "מכשיר סלולארי",
    "store_category_title": "טלפונים",
    "code": "SKU#345",
    "price": "2000"
  }
}

RESPONSE:
{
    "id": "1086782",
    "created_at": "2017-01-18 09:36:52 UTC",
    "updated_at": "2017-01-18 09:36:52 UTC",
    "title": "מכשיר סלולארי",
    "store_category_title": "טלפונים",
    "price": "2000.0",
    "origin_price": "",
    "code": "SKU#345",
    "second_code": "",
    "desc": "",
    "discount_prices": {},
    "related_items": [],
    "images": []
}
```


### עריכת מוצר
#### תיאור
בעזרת שירות זה, ניתן לעדכן מידע למוצר ספציפי.
#### EndPoint
```
PUT /{apiVersion}/items/{id|code|second_code} HTTP/1.1
Host: api.konimbo.co.il
Content-Type: application/json

{
  "token": "{yourToken}",
  "item": {
    "attribute1": "value",
    "attribute2": "value"
  }
}
```
#### שדות שניתן לעדכן למוצר
ראה [פירוט השדות](#user-content-פירוט-השדות)


#### Responses
* 200 - The updated item
* 400 - Bad Request
* 401 - Unauthorized
* 404 - No results found
* 422 - Unprocessable Entity
* 500 - Internal error

#### דוגמאות

```
PUT /v1/items/1086782 HTTP/1.1
Host: api.konimbo.co.il
Content-Type: application/json

{
  "token": "e012a2944be024a85062c2bbd27b9f61284bf2439fd87992069b84b6a4ef93ca",
  "item": {
    "price": "1850",
    "origin_price": "2000"
  }
}

RESPONSE:
{
  "id": "1086782",
  "created_at": "2017-01-18 09:36:52 UTC",
  "updated_at": "2017-01-18 09:52:20 UTC",
  "title": "מכשיר סלולארי",
  "store_category_title": "טלפונים",
  "price": "1850.0",
  "origin_price": "2000.0",
  "code": "SKU#345",
  "second_code": "",
  "desc": "",
  "discount_prices": {},
  "related_items": [],
  "images": []
}
```

### מחיקת מוצר
#### תיאור
בעזרת שירות זה, ניתן להעביר מוצר לפח הזבל.

מוצר אשר נמחק בעזרת שירות זה יהיה ניתן לשחזור.
#### EndPoint
```
DELETE /{apiVersion}/items/{id|code|second_code} HTTP/1.1
Host: api.konimbo.co.il
```
#### Responses
* 200 - Success message
* 400 - Bad Request
* 401 - Unauthorized
* 404 - No results found
* 500 - Internal error

#### דוגמאות
```
DELETE /v1/items/1086782?token=e012a2944be024a85062c2bbd27b9f61284bf2439fd87992069b84b6a4ef93ca HTTP/1.1
Host: api.konimbo.co.il

RESPONSE:
{
  "status": "ok",
  "message": "Item 1086782 has deleted successfully"
}
```
