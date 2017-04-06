# Api Reference
<div dir=rtl>
در مستند زیر می‌توانید نحوه استفاده از api پروژه Echap را مشاهده و از آن استفاده نمایید.
</div>

<div dir=rtl>
توجه کنید که باید به ابتدای تمام آدرس ها عبارت زیر اضافه شود:

<div dir=ltr><b>http://echap.ir/api</b></div>
</div>

## Authorization
### احراز هویت

<div dir="rtl">
در قسمت هایی از API که نیاز به تایید هویت کاربر وجود دارد باید مقدار زیر به هدر اضافه شود:
</div>


```json
{
    "AuthToken": "user_token_here"
}
```


<div dir="rtl">
مقدار این عبارت به هنگام ورود کاربر به سیستم تحویل داده می شود.
</div>

<br>
<div dir="rtl">
درصورتی که این هدر را نفرستید یا مقدار آن اشتباه باشد با خطایی مشابه زیر موجه خواهید شد.
</div>

```json
{
    "success": false,
    "status": 401,
    "error": "Unauthorized. No AuthToken was provided."
}
```

<div dir=rtl>
در صورت اشتباه بودن یا منقضی شدن توکن خطایی مانند زیر میگیرید:
</div>

```json
{
    "success": false,
    "status": 401,
    "error": "Unauthorized. AuthToken is expired or restricted access."
}
```

## User Management

### Login
#### ورود

#### /login
##### method: post

<div dir="rtl">
و موارد زیر را به هدر  درخواست خود اضافه نمایید.(Header)
</div>

```json
{
    "content-type": "application/json",
    "cache-control": "no-cache"
}
```

<div dir="rtl">
دیتای ارسالی به آدرس نیز باید به این شرح باشد.:
</div>

```json
{
    "email": "user_email",
    "password": "user_password"
}
```

<div dir=rtl>
نمونه ی پاسخ موفق:
</div>

```json
{
    "success": true,
    "content": {
        "user": {
            "id": 1,
            "firstName": "John",
            "lastName": "Doe",
            "phoneNumber": "012342342342",
            "phoneNumberConfirmed": false,
            "avatar": "http://localhost:5000/app_data/users/avatars/user_default.png",
            "email": "john.doe@gmail.com",
            "emailConfirmed": false,
            "token": "gsciQ9PQvCpoQUlotv3dM0HJgzDwR6%john.doe@gmail.com",
            "createdAt": "2017-04-05T22:16:00.9486642",
            "userType": {
                "slug": "normal",
                "fullName": "normal",
                "alternativePersianDisplayName": "کاربر عادی",
                "persianDisplayName": "کاربر عادی"
            },
            "addresses": []
        }
    }
}
```

<div dir="rtl">
اگر ایمیل یا کلمه عبور نادرست باشد این خطا را دریافت خواهید کرد:
</div>

```json
{
    "success": false,
    "error": "ایمیل یا کلمه عبور اشتباه است."
}
```

<div dir="rtl">
در غیر این صورت و صحت نام کاربری و کلمه عبور، اطلاعاتی به این صورت دریافت خواهید کرد:
</div>


<div dir="rtl">
از access_token دریافتی میتوانید برای اتصال به api استفاده کنید. نحوه استفاده آن در بخش‌ بعدی توضیح داده خواهد شد.
</div>


### Register
#### ثبت نام
#### /register
##### method: post


<div dir="rtl">
تمام پارامتر های لازم را باید ارسال کنید. مثل:
</div>

```json
{
    "firstName": "John",
    "lastName": "Doe",
    "email": "john.doe@gmail.com",
    "phoneNumber": "09013339665",
    "password": "z4234df",
    "userType": "normal"
}
```

<div dir="rtl">
در صورت وجود خطا، خروجی مثل زیر را دریافت می کنید:
</div>

```json
{
    "success": false,
    "errors": [
        {
            "identifier": "EMAIL_ALREADY_EXISTS",
            "description": "ایمیل قبلا ثبت شده"
        }
    ]
}
```

<div dir=rtl>یا در صورت رخ دادن خطای غیر منتظره در سمت سرور:</div>

```json
{
    "success": false,
    "errors": [
        {
            "identifier": "UNKNOWN_ERROR",
            "description": "متاسفانه، خطایی در هنگام ساخت حساب کاربری رخ داد."
        }
    ]
}
```

### Get List
#### کاربران - دریافت لیست
#### /users/get/{userType}/{page?}
##### method: get

<div dir="rtl">
جهت نمایش گیج‌های ابتدای برنامه از این بخش استفاده کنید.
</div>

###### userType (required)

<div dir="rtl">
این پارامتر ضروری نوع کاربران برگشتی را مشخص میکند. باید یک رشته با حروف لاتین باشد. مثل: press, normal, broker
</div>

###### page

<div dir="rtl">
این پارامتر مشخص می کند که صفحه چندم از لیست برگردانده شود.
</div>

<div dir=rtl>
نمونه پاسخ سرور:
</div>

```json
{
    "success": true,
    "users": [
        {
            "id": 4,
            "firstName": "Eazy",
            "lastName": "E",
            "email": "eazy.e@gmail.com",
            "phoneNumber": "09013269565",
            "password": null,
            "avatar": null,
            "emailConfirmed": false,
            "phoneNumberConfirmed": false,
            "userStatus": null,
            "userType": "چاپخانه",
            "createdAt": "2017-05-05T22:46:00.9486642",
            "addresses": null
        },
        {
            "id": 1,
            "firstName": "John",
            "lastName": "Doe",
            "email": "j.doe@gmail.com",
            "phoneNumber": "09013869560",
            "password": null,
            "avatar": null,
            "emailConfirmed": false,
            "phoneNumberConfirmed": false,
            "userStatus": null,
            "userType": "عادی",
            "createdAt": "2017-04-05T22:16:00.9486642",
            "addresses": null
        }
    ],
    "allCount": 2
}
```

### Get User Type List
#### دریافت نوع های حساب کاربری
#### /users/getallavailableusertypes
##### method: get

<div dir="rtl">
لیستی از تمام انواع کاربران که سیستم پشتیبانی میکند، برمیگرداند.
</div>

```json
[
    {
        "id": 1,
        "slug": "normal",
        "fullName": "normal",
        "alternativePersianDisplayName": "عادی",
        "persianDisplayName": "عادی",
        "enabled": true
    },
    {
        "id": 2,
        "slug": "press",
        "fullName": "press",
        "alternativePersianDisplayName": "چاپخانه",
        "persianDisplayName": "چاپخانه",
        "enabled": true
    },
    {
        "id": 3,
        "slug": "broker",
        "fullName": "broker",
        "alternativePersianDisplayName": "کارگزار",
        "persianDisplayName": "کارگزار",
        "enabled": true
    },
    {
        "id": 4,
        "slug": "technical_office",
        "fullName": "technical_office",
        "alternativePersianDisplayName": "دفتر فنی",
        "persianDisplayName": "دفتر فنی",
        "enabled": true
    }
]
```

### Get User Status List
#### دریافت لیست انواع وضعیت های حساب
#### /users/getallavailableusertypes
##### method: get

<div dir="rtl">
لیستی از تمام وضعیت های ممکن هر حساب کاربری برمیگرداند.
</div>

```json
[
    {
        "evalue": 0,
        "persianDisplayName": "فعال"
    },
    {
        "evalue": 3,
        "persianDisplayName": "غیر فعال"
    },
    {
        "evalue": 1,
        "persianDisplayName": "نیازمند تایید"
    }
]
```

### Search
#### جستوی کاربر
#### /users/search
##### method: get

<div dir=rtl>
با استفاده از این متود می توان بین تمام کاربران جستجو کرد و نتیجه را به صورت صفحه بندی شده دریافت کرد.
<br><br>

##### نمونه اطلاعات جستجوی ارسالی به سرور (query):
</div>

```bash
/users/search?UserFirstLastNameEmailPhoneNumber=John&UserStatus=همه&UserType=عادی
```

<div dir=rtl>
<br>

##### توضیح:

پارامتر های جستجو باید به صورت query string به url درخواست، اضافه شوند. در کل ۴ پارامتر می توان ارسال کرد.
پاسخ به صورت آرایه ای از کاربران خواهد بود.
</div>

###### UserFirstLastNameEmailPhoneNumber
<div dir=rtl>
این پارامتر می تواند حاوی نام، نام خانوادگی، نام و نام خانوادگی، ایمیل یا شماره تلفن همراه کاربر باشد.
</div>

###### UserStatus
<div dir=rtl>
با اسفاده از این پارامتر می توان جستجو را بین کاربرانی با وضعیتی خاص محدود کرد.
در صورت قرار دادن کلمه <b>همه</b> به عنوان این پارامتر، عملا این پارامتر در نظر گرفته نمیشود.
</div>

###### UserType
<div dir=rtl>
با اسفاده از این پارامتر می توان جستجو را بین کاربرانی با نوع خاص محدود کرد.
در صورت قرار دادن کلمه <b>همه</b> به عنوان این پارامتر، عملا این پارامتر در نظر گرفته نمیشود.
</div>

###### UserRegisteredAt (NMT)
<div dir=rtl>
این پارامتر جستجو را بین کاربرانی که در یک تاریخ خاص ثبت نام کردند، محدود میکند.
</div>

<br>

### Get
#### دریافت کاربر
#### /users/get/{id}
##### method: get

<div dir="rtl">
این متود مشخصات یک کاربر خاص که با پارامتر id مشخص شده است را بر میگرداند.
<br><br>
نمونه پاسخ دریافتی از سرور:
</div>

```json
{
    "success": true,
    "user": {
        "id": 1,
        "firstName": "John",
        "lastName": "Doe",
        "email": "j.doe@gmail.com",
        "phoneNumber": "09013869560",
        "password": null,
        "avatar": "http://example.com/path/to/file/user_default.png",
        "emailConfirmed": false,
        "phoneNumberConfirmed": false,
        "userStatus": "فعال",
        "userType": "عادی",
        "createdAt": "2017-04-05T22:16:00.9486642",
        "addresses": []
    }
}
```

###### id
<div dir=rtl>id کاربر درخواستی</div>
<br>
<div dir=rtl>
توجه کنید که مقدار کلمه عبور واقعی کاربر هیچگاه برگردانده نخواهد شد و در پاسخ سرور مقدار null قرار داده خواهد شد.
</div>


### Edit [[`admin`]]
#### ویرایش کاربر
#### /users/edit/{id}
##### method: post

<div dir="rtl">
با استفاده از این متود، امکان ایجاد تغییرات در پروفایل کاربران فراهم میشود. این متود یک پارامتر در URL دریافت می کند که عبارت است از id کاربر مورد نظر.
</div>

###### id:
<div dir=rtl>id کاربر مورد نظر</div>
<br>
<div dir=rtl>
نمونه اطلاعات ارسال شده به سرور:
</div>

```json
{
    "firstName": "تست",
    "lastName": "تستر پور",
    "email": "tester@gmail.com",
    "phoneNumber": "09013269565",
    "password": null,
    "userStatus": "نیازمند تایید",
    "userType": "عادی"
}
```

<div dir=rtl>
به جز فیلد password باقی فیلد ها ضروری هستند و حتما باید ارسال شوند. در صورت خالی بودن فیلد password، کلمه عبور کاربر تغییر نمی کند.
</div>

### Generate Header Menu
#### ایجاد منوی بالای صفحه
#### /shared/generateheadermenu
##### method: get
<div dir="rtl">
جهت دریافت منوی بالای صفحه، شامل دسته بندی ها،سرویس های آن ها و عکس های مربوط، از این بخش استفاده کنید.
</div>

```json
[
    {
        "id": 1,
        "category": "کارت ویزیت",
        "category_en": null,
        "categoryicon": "http://example.com/assets/images/icons/cat-1.png",
        "services": [
            {
                "id": 32,
                "servicename": "سلفون مات",
                "service_en": "hardcover-opaque",
                "servicephoto": "http://example.com/path/to/file/1.jpg"
            },
            {
                "id": 31,
                "servicename": "سلفون براق",
                "service_en": null,
                "servicephoto": "http://example.com/path/to/file/2.jpg"
            }
        ]
    },
    {
        "id": 2,
        "category": "اداری",
        "category_en": null,
        "categoryicon": "http://example.com/assets/images/icons/cat-2.png",
        "services": []
    }
]
```

### Generate Main Search Menu
#### ایجاد منوی جستجوی اصلی
#### /shared/generatemainsearchmenu
##### method: get
<div dir="rtl">
جهت دریافت اطلاعات منوی جستجو در صفحه اصلی برنامه شامل سرویس ها، شهر ها، و محلات از این بخش استفاده کنید.
</div>

```json
{
    "categoryAndServices": [
        {
            "serviceId": 2,
            "serviceTitle": "گلاسه UV",
            "categoryId": 1,
            "categoryTitle": "کارت ویزیت"
        },
        {
            "serviceId": 3,
            "serviceTitle": "PVC 300 میکرون",
            "categoryId": 1,
            "categoryTitle": "کارت ویزیت"
        }
    ],
    "cities": [
        {
            "id": 1,
            "name": "تهران",
            "slug": "tehran",
            "areas": [
                {
                    "id": 1,
                    "name": "آجودانیه",
                    "slug": null
                },
                {
                    "id": 2,
                    "name": "خرمشهر",
                    "slug": null
                },
                {
                    "id": 3,
                    "name": "خواجه نصیر طوسی",
                    "slug": null
                }
            ]
        }
    ]
}
```



## limit (optional) {gauges-v2}

<div dir="rtl">
تعداد رکوردهای مربوط به هر صفحه را نمایش می‌دهد. می‌توانید مشخص کنید که در هربار واکشی اطلاعات چه تعداد رکورد را برای شما بیاورد.
</div>

default: 10

## search (optional) {gauges-v2}

<div dir="rtl">
برای جستجوی عبارت خاص در رکوردها از این پارامتر استفاده کنید. این پارامتر جهت استفاده در بخش‌های مختلف جستجو در دسترس است و در آدرس‌های مختلف به همین منظور قرار داده‌شده‌است.
</div>

default: ""

## Sample  {gauges-v2}

<div dir="rtl">
نمونه نحوه ارسال درخواست با OkHttpClient در جاوا
</div>

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://armandar.com/api/v۲/gauges/1395/1/1/10/گیلان")
  .get()
  .addHeader("authorization", "Bearer gyXOAl0G4lg5LQYDa....")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```


<div dir="rtl">
و نتیجه دریافتی به صورت زیر خواهد بود.
</div>

```json
[
    {
        "Id" = 12, //برای دریافت زیرمجموعه‌ها از این Id استفاده کنید.
        "Value": 40.66,
        "ScheduleValue": 97.5,
        "YearId": 1395,
        "DeviationFactor": -56.84,
        "OldScheduleValue": 0,
        "HeadOrganizationChartId": 69,
        "Title": "گیلان"
    }
]
```

Title: عنوان

Value: پیشرفت واقعی

ScheduleValue: پیشرفت برنامه‌ای

DeviationFactor: میرزان انحراف


<div dir="rtl">
بقیه اطلاعات نیز در حال حاضر مورد نیاز نیست و جایی نمایش داده نمی‌شود.
</div>





# Projects
### url: armandar.com/api/v1/projects/{year}/{page?}/{limit?}/{search?}  --- method: get

<div dir="rtl">
جهت نمایش پروژه‌ها از این بخش استفاده کنید.
</div>


## year (required) {projects}

<div dir="rtl">
سال جاری که کاربر در بخش تنظیمات مشخص نموده‌است با این درخواست و به عنوان پارامتر اول ارسال شود.
</div>

## page (optional) {projects}

<div dir="rtl">
با توجه به اینکه میزان اطلاعات ممکن است زیاد شود و هم زمان لود طول بکشد و هم UI از زیبایی خارج شود اطلاعات به صورت صفحه به صفحه می‌اید. این پارامتر شماره صفحه را برای دریافت اطلاعات آن صفحه مشخص می‌کند.
</div>

default:1

## limit (optional) {projects}

<div dir="rtl">
تعداد رکوردهای مربوط به هر صفحه را نمایش می‌دهد. می‌توانید مشخص کنید که در هربار واکشی اطلاعات چه تعداد رکورد را برای شما بیاورد.
</div>

default: 10

## search (optional) {projects}

<div dir="rtl">
برای جستجوی عبارت خاص در رکوردها از این پارامتر استفاده کنید. این پارامتر جهت استفاده در بخش‌های مختلف جستجو در دسترس است و در آدرس‌های مختلف به همین منظور قرار داده‌شده‌است.
</div>

default: ""

## Sample  {projects}

<div dir="rtl">
نمونه نحوه ارسال درخواست با OkHttpClient در جاوا
</div>

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://armandar.com/api/v1/projects/1395/1/10/گیلان")
  .get()
  .addHeader("authorization", "Bearer gyXOAl0G4lg5LQYDa....")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```


<div dir="rtl">
و نتیجه دریافتی به صورت زیر خواهد بود.
</div>

```json
[
    {
        "Id": 2317,
        "Code": "P95-002317",
        "Title": "تست ۱",
        "OrganizationChartId": 690809,
        "OrganizationChartTitle": "گیلان",
        "StatusId": 2,
        "StatusTitle": "در حال انجام",
        "ScheduleValue": 98.76,
        "Value": 86,
        "YearId": 1395,
        "DeviationFactor": -12.76
    },
    {
        "Id": 2371,
        "Code": "P95-002371",
        "Title": "تست ۲",
        "OrganizationChartId": 690809,
        "OrganizationChartTitle": "گیلان",
        "StatusId": 2,
        "StatusTitle": "در حال انجام",
        "ScheduleValue": 99.75,
        "Value": 60,
        "YearId": 1395,
        "DeviationFactor": -39.75
    },...
]
```

Title: عنوان

Code: کد (حتماً کد را هم نمایش دهید)

OrganizationChartTitle: نام دانشگاه یا واحد

Value: پیشرفت واقعی

ScheduleValue: پیشرفت برنامه‌ای

DeviationFactor: میرزان انحراف

StatusTitle: وضعیت

<div dir="rtl">
بقیه اطلاعات نیز در حال حاضر مورد نیاز نیست و جایی نمایش داده نمی‌شود.
</div>




# Projects (Version 2)
### url: armandar.com/api/v2/projects/{year}/{page?}/{limit?}/{alerttype?}/{search?}  --- method: get

<div dir="rtl">

> تذکر: ابتدا [به روز رسانی مربوط به Alert](#alerts-updated-alert)  را بخوانید. 


جهت نمایش پروژه‌ها از این بخش استفاده کنید. تمام اطلاعات مانند ورژن اول می‌باشد. تنها یک آپشن جدید alerttype جهت نمایش پروژه‌های مربوط به هشدارها اضافه شده‌است.
</div>


## year (required) {projects-v2}

<div dir="rtl">
سال جاری که کاربر در بخش تنظیمات مشخص نموده‌است با این درخواست و به عنوان پارامتر اول ارسال شود.
</div>

## page (optional) {projects-v2}

<div dir="rtl">
با توجه به اینکه میزان اطلاعات ممکن است زیاد شود و هم زمان لود طول بکشد و هم UI از زیبایی خارج شود اطلاعات به صورت صفحه به صفحه می‌اید. این پارامتر شماره صفحه را برای دریافت اطلاعات آن صفحه مشخص می‌کند.
</div>

default:1

## limit (optional) {projects-v2}

<div dir="rtl">
تعداد رکوردهای مربوط به هر صفحه را نمایش می‌دهد. می‌توانید مشخص کنید که در هربار واکشی اطلاعات چه تعداد رکورد را برای شما بیاورد.
</div>

default: 10

## alerttype (optional) {projects-v2}

<div dir="rtl">
وقتی کاربر روی هشدارها کلیک کرده و مقدار EnTitle مربوط به هشدار کلیک شده را گرفتید همان را به عنوان alerttype باید به این متد ارسال کنید.


> نکته: دقت کنید که حتماً مقدار CanClick آیتم کلیک شده برابر با true باشد.


جهت اطمینان مقادیر مجاز برای alerttype که مقدار null بر نمی‌گرداند به شرح زیر است.
</div>

- AlertCount
- ProjectStepIssueCount
- ProjectWFIssueCount
- ProjectChallengeIssueCount
- ProjectGoalIssueCount
- و مقدار خالی که عملکرد آن مشابه [نسخه اول این متد](#projects) خواهد شد.

default: ""

## search (optional) {projects-v2}

<div dir="rtl">
برای جستجوی عبارت خاص در رکوردها از این پارامتر استفاده کنید. این پارامتر جهت استفاده در بخش‌های مختلف جستجو در دسترس است و در آدرس‌های مختلف به همین منظور قرار داده‌شده‌است.
</div>

default: ""

## Sample  {projects-v2}

<div dir="rtl">
نمونه نحوه ارسال درخواست با OkHttpClient در جاوا
</div>

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://armandar.com/api/v2/projects/1395/1/10/AlertCount/گیلان")
  .get()
  .addHeader("authorization", "Bearer gyXOAl0G4lg5LQYDa....")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```


<div dir="rtl">
و نتیجه دریافتی به صورت زیر خواهد بود.
</div>

```json
[
    {
        "Id": 2317,
        "Code": "P95-002317",
        "Title": "تست ۱",
        "OrganizationChartId": 690809,
        "OrganizationChartTitle": "گیلان",
        "StatusId": 2,
        "StatusTitle": "در حال انجام",
        "ScheduleValue": 98.76,
        "Value": 86,
        "YearId": 1395,
        "DeviationFactor": -12.76
    },
    {
        "Id": 2371,
        "Code": "P95-002371",
        "Title": "تست ۲",
        "OrganizationChartId": 690809,
        "OrganizationChartTitle": "گیلان",
        "StatusId": 2,
        "StatusTitle": "در حال انجام",
        "ScheduleValue": 99.75,
        "Value": 60,
        "YearId": 1395,
        "DeviationFactor": -39.75
    },...
]
```

Title: عنوان

Code: کد (حتماً کد را هم نمایش دهید)

OrganizationChartTitle: نام دانشگاه یا واحد

Value: پیشرفت واقعی

ScheduleValue: پیشرفت برنامه‌ای

DeviationFactor: میرزان انحراف

StatusTitle: وضعیت


<div dir="rtl">
بقیه اطلاعات نیز در حال حاضر مورد نیاز نیست و جایی نمایش داده نمی‌شود. در صورتی که مقدار alerttype چیزی به جز رشته خالی باشد مقدار پیشرفت‌ها و انحراف‌ها همگی صفر بر‌می‌گردد چون کاربردی ندارند.
</div>






# Project Steps
### url: armandar.com/api/v1/steps/{project}/{search?}  --- method: get

<div dir="rtl">
جهت نمایش فعالیت‌های یک پروژه وفتی روی آن کلیک می‌شود از این بخش استفاده کنید. شناسه پروژه برای دریافت اطلاعات باید ارسال شود. در این متد ما به سال نیاز نداریم

با توجه به اینکه تعداد فعالیت‌ها معمولا خیلی زیاد نیست در این بخش ما صفحه‌بندی نداریم و تمام اطلاعات با یک درخواست قابل دریافت است.

</div>

>  با توجه به اینکه فعالیت‌ها ساختار درختی دارند به جیسان دریافتی دقت نموده و برای نمایش با ساختاری مشابه درختی تدبیری بیندیشید.



## project (required) {steps}

<div dir="rtl">
شناسه پروژه‌ای که کاربر انتخاب نموده است با استفاده از پارامتر اول ارسال شود.
</div>


## search (optional) {steps}

<div dir="rtl">
برای جستجوی عبارت خاص در رکوردها از این پارامتر استفاده کنید. این پارامتر جهت استفاده در بخش‌های مختلف جستجو در دسترس است و در آدرس‌های مختلف به همین منظور قرار داده‌شده‌است.
</div>

default: ""

## Sample  {steps}

<div dir="rtl">
نمونه نحوه ارسال درخواست با OkHttpClient در جاوا
</div>

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://armandar.com/api/v1/steps/642")
  .get()
  .addHeader("authorization", "Bearer gyXOAl0G4lg5LQYDa....")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```


<div dir="rtl">
و نتیجه دریافتی به صورت زیر خواهد بود.
</div>

```json
[
    {
        "Id": 3105,
        "ProjectId": 642,
        "Title": "بررسی وضعیت موجود و تعداد نیروی انسانی شاغل",
        "WeightFactor": 20,
        "ParentId": null,
        "Level": 1,
        "Value": 90,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    },
    {
        "Id": 31272,
        "ProjectId": 642,
        "Title": "تست1",
        "WeightFactor": 100,
        "ParentId": 3105,
        "Level": 2,
        "Value": 0,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    },
    {
        "Id": 3106,
        "ProjectId": 642,
        "Title": "استخراج آخرین استانداردهای نیروی انسانی",
        "WeightFactor": 40,
        "ParentId": null,
        "Level": 1,
        "Value": 75,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    },
    {
        "Id": 3103,
        "ProjectId": 642,
        "Title": "بررسی و مقایسه وضعیت موجود با استانداردها",
        "WeightFactor": 20,
        "ParentId": null,
        "Level": 1,
        "Value": 85,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    },
    {
        "Id": 3108,
        "ProjectId": 642,
        "Title": "ارائه راه حل برای جبران کمبودها",
        "WeightFactor": 20,
        "ParentId": null,
        "Level": 1,
        "Value": 40,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    },
    {
        "Id": 31273,
        "ProjectId": 642,
        "Title": "ururt",
        "WeightFactor": 100,
        "ParentId": 3108,
        "Level": 2,
        "Value": 0,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    },
    {
        "Id": 31274,
        "ProjectId": 642,
        "Title": "ururt",
        "WeightFactor": 80,
        "ParentId": 31273,
        "Level": 3,
        "Value": 80,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    },
    {
        "Id": 31275,
        "ProjectId": 642,
        "Title": "تست1",
        "WeightFactor": 20,
        "ParentId": 31273,
        "Level": 3,
        "Value": 80,
        "ScheduleValue": 0,
        "DeviationFactor": 0
    }
]
```

Title: عنوان

Value: پیشرفت

Level: سطح


<div dir="rtl">
بقیه اطلاعات نیز در حال حاضر مورد نیاز نیست و جایی نمایش داده نمی‌شود.


مقدار Level سطح تو رفتگی را در درخت نمایش خواهد داد.
</div>


# Actions
### url: armandar.com/api/v1/actions/{year}/{page?}/{limit?}/{search?}  --- method: get

<div dir="rtl">
برای نمایش لیست اقدامات جاری کاربر از این متد استفاده کنید.
</div>


## year (required) {actions}

<div dir="rtl">
سال جاری که کاربر در بخش تنظیمات مشخص نموده‌است با این درخواست و به عنوان پارامتر اول ارسال شود.
</div>

## page (optional) {actions}

<div dir="rtl">
با توجه به اینکه میزان اطلاعات ممکن است زیاد شود و هم زمان لود طول بکشد و هم UI از زیبایی خارج شود اطلاعات به صورت صفحه به صفحه می‌اید. این پارامتر شماره صفحه را برای دریافت اطلاعات آن صفحه مشخص می‌کند.
</div>

default:1

## limit (optional) {actions}

<div dir="rtl">
تعداد رکوردهای مربوط به هر صفحه را نمایش می‌دهد. می‌توانید مشخص کنید که در هربار واکشی اطلاعات چه تعداد رکورد را برای شما بیاورد.
</div>

default: 10

## search (optional) {actions}

<div dir="rtl">
برای جستجوی عبارت خاص در رکوردها از این پارامتر استفاده کنید. این پارامتر جهت استفاده در بخش‌های مختلف جستجو در دسترس است و در آدرس‌های مختلف به همین منظور قرار داده‌شده‌است.
</div>

default: ""


## Sample  {actions}

<div dir="rtl">
نمونه نحوه ارسال درخواست با OkHttpClient در جاوا
</div>

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://armandar.com/api/v1/actions/1395")
  .get()
  .addHeader("authorization", "Bearer gyXOAl0G4lg5LQYDa....")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```


<div dir="rtl">
و نتیجه دریافتی به صورت زیر خواهد بود.
</div>

```json
[
  {
    "Id": 2366,
    "Title": "تهیه گزارش‌های ماهانه سال 95",
    "Code": "A95-002366",
    "YearId": 1395,
    "OrganizationChartTitle": "معاونت توسعه مدیریت و منابع ",
    "Value": 36.84,
    "ScheduleValue": 92.62
  },
  {
    "Id": 2707,
    "Title": "تهیه و انتشار ماهنامه تست",
    "Code": "A95-002707",
    "YearId": 1395,
    "OrganizationChartTitle": "دانشگاه تهران",
    "Value": 62.5,
    "ScheduleValue": 90.83
  }
]
```

Title: عنوان

Code: کد (حتماً نمایش داده شود.)

OrganizationChartTitle: نام دانشگاه یا واحد

Value: پیشرفت

ScheduleValue: پیشرفت برنامه‌ای

<div dir="rtl">
بقیه اطلاعات نیز در حال حاضر مورد نیاز نیست و جایی نمایش داده نمی‌شود.
</div>

<div dir="rtl">
</div>


# Save Step Progresses

### url: armandar.com/api/v2/saveprogress --- method: post

<div dir="rtl">
به آدرس بالا درخواست زده 
و موارد زیر را به هدر  درخواست خود اضافه نمایید.(Header)
</div>

```json
headers: {
    "content-type": "application/x-www-form-urlencoded",
    "cache-control": "no-cache",
    "Authorization": "Bearer r6iBDo8NCynu8-VON9E924qrom4gA..."
}
```

<div dir="rtl">
دیتای ارسالی به آدرس نیز باید به این شرح باشد.:
</div>

```json
data: {
    "ProjectStepId": 12, //int
    "DatePersian": "1395/01/01", //string
    "Progress": 10.25 //double
}
```

<div dir="rtl">
نمونه کد ارسالی با استفاده از OkHttpClient در جاوا به صورت زیر است:
</div>

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
RequestBody body = RequestBody.create(mediaType, "ProjectStepId=12&DatePersian=1395%2F01%2F01&Progress=10.25");
Request request = new Request.Builder()
  .url("http://armandar.com/api/v2/saveprogress")
  .post(body)
  .addHeader("authorization", "Bearer r6iBDo8NCynu8-VON9E924qrom4gA....")
  .addHeader("content-type", "application/x-www-form-urlencoded")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```

<div dir="rtl">
اگر اطلاعات ارسال دچار مشکل باشد اطلاعاتی به شکل زیر دریافت می‌کنید که میتوانید پیام موجود را به صورت هشدار به کاربر نمایش دهید.:
</div>

```json
{
    "Id": 0,
    "Success": false,
    "Message": "برای تاریخ مورد نظر اطلاعات ثبت شده است. لطفا تاریخ را تصحیح نمایید.",
    "Data": null
}
```

<div dir="rtl">
در غیر این صورت و اطلاعاتی به این صورت دریافت خواهید کرد:
</div>

```json
{
  "Id": 12,
  "Success": true,
  "Message": "ذخیره اطلاعات با موفقیت انجام شد.",
  "Data": {
    "NewActualProgress": 22.2,
    "NewScheduleProgress": 99.33
  }
}
```


<div dir="rtl">
مقادیر NewActualProgress و NewScheduleProgress را در صورتی که بخواهید همان لحظه نمودار پیشرفت پروژه‌ی مربوط به این فعالیت را بروزرسانی کنید می‌توانید مورد استفاده قرار دهید.
</div>
