# Api Reference
<div dir=rtl>
در مستند زیر می‌توانید نحوه استفاده از API پروژه Echap را مشاهده و از آن استفاده نمایید.
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

### - Login
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


### - Register
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

### - Get List
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

### - Get User Type List
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

### - Get User Status List
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

### - Search
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

### - Get
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


### - Edit [[`admin`]]
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

### - Generate Header Menu
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

### - Generate Main Search Menu
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
