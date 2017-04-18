# API Reference
<div dir=rtl>
در مستند زیر می‌توانید نحوه استفاده از API پروژه Echap را مشاهده و از آن استفاده نمایید.
</div>

<div dir=rtl>

- توجه کنید که باید به ابتدای تمام آدرس ها عبارت زیر اضافه شود:
<div dir=ltr><b>http://echap.ir/api</b></div>

- علامت [[]] در روبروی برخی از متود ها به معنی اعمال محدودیت دسترسی می باشد. مثلا [[currentUser]] به معنی آن است که به طور پیش فرض این متد فقط برای کاربرانی قابل دسترسی است که وارد سیستم شده باشند.

- حساب کاربری admin **همیشه** به **تمام** متد ها دسترسی دارد و به همین خاطر اسم آن جلوی متد ها قرار داده نشده است.
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
            "credit": 15000,
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
این متد لیستی از کاربران را بر میگرداند.
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
            "credit": 5000,
            "userType": "چاپخانه",
            "createdAt": "2017-05-05T22:46:00.9486642"
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
            "credit": 0,
            "userType": "عادی",
            "createdAt": "2017-04-05T22:16:00.9486642"
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
        "emailConfirmed": false,
        "phoneNumber": "09013869560",
        "phoneNumberConfirmed": false,
        "userType": "عادی",
        "userStatus": "فعال",
        "creadit" 0,
        "avatar": "http://example.com/path/to/file/user_default.png",
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


### - Edit [[]]
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


### - Delete [[]]
#### حذف کاربر
#### /users/delete/{id}
##### method: post

<div dir="rtl">
با استفاده از این متود، امکان حذف حساب کاربران فراهم میشود. این متود یک پارامتر در URL دریافت می کند که عبارت است از id کاربر مورد نظر.
<br><br>
<b>توجه کنید که حتما باید مثل متود Edit اطلاعات پروفایل کاربر هم به سرور ارسال شود.</b>
</div>

###### id:
<div dir=rtl>id کاربر مورد نظر</div>

### - Upload Avatar [[]]
#### آپلود آواتار
#### /users/uploadavatar/{id}
##### method: post

<div dir="rtl">
این متود با دریافت id کاربر در URL و همچنین یک عکس با فرمت های JPG و PNG آواتار کاربر را تعیین می کند.
</div>

###### id:
<div dir=rtl>id کاربر مورد نظر</div>

<div dir=rtl>
توجه کنید که فیلد عکس فرستاده شده به سمت سیستم باید در فرم دارای نام avatar باشد.
<br><br>
نمونه پاسخ سرور:
</div>

```json
{
    "success": true,
    "userId": 5,
    "avatarFullPath": "http://example.com/path/to/file/54ff8864d1f9671d156ff65e49075499.png"
}
```

### - Add Address [[currentUser]]
#### اضافه کردن آدرس
#### /users/addaddress/{userId}
##### method: get

<div dir="rtl">
برای اضافه کردن آدرس به پروفایل کاربر از این متود استفاده کنید. اطلاعات آدرس حاوی نام آدرس و متن باید به صورت query string به URL اضافه شوند.
<br><br>

##### نمونه درخواست ارسالی به سرور:
</div>

```bash
/users/addaddress?Title=محل کار&Text=آدرس کامل در این قسمت
```

###### userId:
<div dir=rtl>id کاربر مورد نظر</div>

<div dir=rtl>
<br>
نمونه پاسخ سرور در حالت خطا:
</div>

```json
{
    "success": false,
    "error": "تمامی فیلد ها باید پر شوند."
}
```

### - Delete Address [[currentUser]]
#### حذف آدرس
#### /users/deleteaddress/{userId}
##### method: get

<div dir="rtl">
برای حذف یک آدرس از پروفایل کاربر باید تمام اطلاعات مرتبط با آدرس را که در اختیار دارید مثل id و عنوان و متن آدرس را در قالب query string به این متود ارسال کنید.
<br><br>

##### نمونه درخواست ارسالی به سرور:
</div>

```bash
/users/addaddress?Id=4&Title=محل کار&Text=آدرس کامل در این قسمت
```

###### userId:
<div dir=rtl>id کاربر مورد نظر</div>

<div dir=rtl>
<br>
پاسخ سرور مشابه متود Add Address خواهد بود.
</div>

## Generate Header Menu
### ایجاد منوی بالای صفحه
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

## Generate Main Search Menu
### ایجاد منوی جستجوی اصلی
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

## Generate user dashboard menu [[currentUser]]
### ایجاد منوی داشبورد کاربر
#### /shared/generatedashboardmenu
##### method: get
<div dir="rtl">
با توجه به اینکه آیتم های داخل منوی داشبورد اصلی ممکن است با توجه به نوع کاربر وارد شده به سیستم متفاوت باشد، از این متود 
برای دریافت جزییات آیتم های مخصوص کاربر وارد شده به سیستم استفاده می شود.
</div>

```json
[
    {
        "title": "داشبورد",
        "router": "/dashboard/home",
        "icon": "dashboard"
    },
    {
        "title": "کاربران",
        "router": "/dashboard/users",
        "icon": "account_box",
        "children": [
            {
                "title": "عادی",
                "router": "/dashboard/users?filter=normal"
            },
            {
                "title": "چاپخانه",
                "router": "/dashboard/users?filter=press"
            },
            {
                "title": "کارگزار",
                "router": "/dashboard/users?filter=broker"
            },
            {
                "title": "دفتر فنی",
                "router": "/dashboard/users?filter=technical_office"
            }
        ]
    },
    {
        "title": "چاپخانه ها",
        "router": "/dashboard/presses",
        "icon": "print"
    },
    {
        "title": "راهنما",
        "router": "/dashboard/help",
        "icon": "help"
    },
    {
        "title": "حساب کاربری",
        "router": "/dashboard/profile",
        "icon": "account_box"
    },
    {
        "title": "خروج",
        "router": "/dashboard/logout",
        "icon": "account_box"
    }
]
```

## Search and Order

### - Advanced Product Search
#### جستجوی پیشرفته بین محصولات
#### /search/advanced
##### method: get
<div dir="rtl">
این متود امکان جستجو بین محصولات موجود در سیستم را به همراه مشخص کردن پارامتر های مختلف می دهد. تمام پارامتر ها باید بصورت query string به URL اضافه شوند.

توجه کنید که برخی از پارامتر های پاسخ مثل averagePrice صرفا زمانی محاسبه می شوند که براساس آن ها sort یا filter درخواست شود.
<br><br>

##### نمونه درخواست ارسالی به سرور:
</div>

```bash
/search/advanced?service=کارت ویزیت - گلاسه معمولی&city=تهران&area=مدنی&coating=1&corners=undefined&printedSide=undefined&folding=undefined&printsize=2&quantity=1&groomet=undefined&color=undefined&sortBy=undefined
```

<div dir=rtl>
امکان مرتب سازی/فیلتر نتایج براساس پارامتر هم وجود دارد. کافیست نام آن پارامتر را به عنوان مقدار عبارت زیر به query string جستجو اضافه کنید:
</div>

```bash
sortBy=recommended
```

<div dir=rtl>
پاسخ سرور عبارت است از آرایه ای از press ها که هر کدام از اجزای این آرایه حاوی اطلاعات کامل آن press و سرویس های قابل ارایه و ... است:
</div>

```json
[
    {
        "pressId": 4,
        "pressName": "تدبیر",
        "pressLogo": "http://example.com/path/to/file/press_default.jpg",
        "pressRate": 5,
        "pressSampleImage": null,
        "pressRecommended": false,
        "pressArea": "مدنی از امام علی تا امام حسین",
        "pressGeoLocation": {
            "latitude": 35.714392058909,
            "longitude": 51.46107673645,
            "zoomLevel": 0,
            "geoLocationType": 1
        },
        "serviceId": 4,
        "serviceName": "کارت ویزیت",
        "averagePrice": 0,
        "averageTurnAround": 0,
        "pressExtraFeatures": [
            {
                "id": 2,
                "title": "ماشین آلات مدرن",
                "description": null,
                "slug": null,
                "image": null,
                "enabled": true,
                "showOrder": 1,
                "press": null
            }
        ],
        "pressDatasList": [
            {
                "id": 1,
                "coatingId": 1,
                "printSizeId": 1,
                "quantityId": 1,
                "printedSideId": null,
                "cornerId": null,
                "colorId": null,
                "foldingId": null,
                "groometId": null,
                "turnAround": 3,
                "cost": 50000,
                "pressService": null
            },
            {
                "id": 2,
                "coatingId": 1,
                "printSizeId": 1,
                "quantityId": 1,
                "printedSideId": null,
                "cornerId": null,
                "colorId": null,
                "foldingId": null,
                "groometId": null,
                "turnAround": 5,
                "cost": 30000,
                "pressService": null
            }
        ]
    }
]
```

### - Get Orders list [[currentUser]]
#### لیست سفارش ها
#### /order/get/{orderStatus}/{page}
##### method: get

<div dir="rtl">
این متود با توجه به کاربری که وارد سیستم شده است لیستی از سفارش های مرتبط با آن حساب کاربری را برمیگرداند. 
مثلا برای کاربر عادی، سفارش های ثبت شده توسط آن کاربر و برای یک چاپخانه، سفارش های ثبت شده برای آن چاپخانه را بر میگرداند.
<br><br>
پاسخ سرور به صورت صفحه بندی شده است که هر صفحه حداکثر ۱۰ نتیجه خواهد داشت. در ضمن نتایج از جدیدترین به قدیمی ترین به ترتیب در صفحه اول و آخر قرار خواهند گرفت.
<br>
</div>

###### orderStatus (required)
<div dir="rtl">
این پارامتر در صورتی که مقدارش برابر all باش، سفارش ها را صرف نظر از وضعیت بر میگرداند.
در صورت تمایل برای نمایش فقط سفارش هایی در وضعیت خاص، باید نام فارسی آن وضعیت را ارسال کنید. مثل "سبد خرید"
</div>

###### page:
<div dir=rtl>صفحه درخواست</div>

<br>
<div dir=rtl>

##### نمونه پاسخ سرور:
</div>

```json
{
    "success": true,
    "orders": [
        {
            "id": 2,
            "status": "سبد خرید",
            "date": "2017-05-03T00:00:00",
            "pressId": 4,
            "pressName": "تدبیر",
            "pressDataCost": 50000,
            "pressDataTurnAround": 3,
            "attachments": null,
            "userId": 1,
            "userFullName": "وحید امیری مطلق",
            "pressData": {
                "serviceId": 2,
                "serviceTitle": "گلاسه UV"
            }
        },
        {
            "id": 3,
            "status": "در حال بررسی",
            "date": "2016-08-14T00:00:00",
            "pressId": 12,
            "pressName": "آذین چاپ",
            "pressDataCost": 35000,
            "pressDataTurnAround": 1,
            "attachments": null,
            "userId": 1,
            "userFullName": "وحید امیری مطلق",
            "pressData": {
                "serviceId": 2,
                "serviceTitle": "گلاسه UV"
            }
        }
    ],
    "allCount": 2
}
```

### - Get Order by id [[currentUser]]
#### دریافت اطلاعات کامل یک سفارش
#### /order/getbyid/{id}
##### method: get

<div dir="rtl">
این متود اطلاعات کامل مربوط به یک سفارش را بر میگرداند. فقط قابل دسترسی برای کاربر ثبت کننده و ادمین می باشد.
<br><br>
</div>

###### id:
<div>id سفارش مورد نظر</div>

<div dir=rtl>

##### نمونه پاسخ سرور:
</div>

```json
{
  "success": true,
  "order": {
    "id": 2,
    "isPaid": false,
    "date": "2017-05-03T00:00:00",
    "status": "سبد خرید",
    "pressDataCost": 50000,
    "pressDataTurnAround": 3,
    "user": {
      "id": 1,
      "fullName": "وحید امیری مطلق",
      "email": "vahid.a1996@gmail.com",
      "phoneNumber": "09013269565"
    },
    "press": {
      "id": 4,
      "name": "تدبیر",
      "logo": null,
      "userId": null,
      "userFullName": null
    },
    "attachments": [
      {
        "id": 2,
        "name": "4353453275984.png"
      }
    ],
    "pressData": {
      "serviceId": 2,
      "serviceTitle": "گلاسه UV"
    },
    "orderMessaging": [
      {
        "id": 1,
        "userId": 1,
        "userFullName": "وحید امیری مطلق",
        "userEmail": "vahid.a1996@gmail.com",
        "userPhoneNumber": "09013269565",
        "comment": "لطفا به دقت انجام شود.",
        "date": "2016-05-16T00:00:00"
      }
    ]
  }
}
```

### - Cancel Order [[currentUser]]
#### لغو سفارش
#### /order/cancelorder/{orderId}
##### method: get

<div dir="rtl">
این متود وضعیت یک سفارش را به لغو شده تغییر می دهد. فقط قابل دسترسی برای admin و کاربری که سفارش را ثبت کرده است می باشد.
<br><br>

##### نمونه درخواست ارسالی به سرور:
</div>

```bash
/search/advanced?service=کارت ویزیت - گلاسه معمولی&city=تهران&area=مدنی&coating=1&corners=undefined&printedSide=undefined&folding=undefined&printsize=2&quantity=1&groomet=undefined&color=undefined&sortBy=undefined
```

###### orderId:
<div dir=rtl>id سفارش مورد نظر</div>

### - Submit Order [[currentUser]]
#### ثبت سفارش
#### /order/submit
##### method: post


<div dir="rtl">
این متود آرایه ای حاوی id های press data انتخابی کاربر را دریافت می کند و یک سفارش ثبت میکند.
</div>

```json
{
    "pressDatasIds": [1, 4],
}
```

<div dir="rtl">
محتوای پاسخ سرور در صورت موفقیت آمیز بودن:
</div>

```json
{
    "success": true,
    "userId": 1,
    "orderIds": [7, 8]
}
```

<div dir=rtl>
orderId باید از این پاسخ در جایی نگه داری شود تا در مرحله بعدی برای ارسال فایل های طرح چاپ از آن استفاده شود.
</div>


### - Get Artwork Sides
#### دریافت لیست بخش های مختلف فایل های طرح چاپ
#### /order/getartworksides/{serviceId}
##### method: get

<div dir="rtl">
این متد یک پارامتر دریافت می کند که همان کد سرویس مورد نظر است. دلیل آن هم این است که سرویس های متفاوت ممکن است احتیاج به فایل های متفاتی برای سمت های مختلف خود داشته باشند.
با استفاده از پاسخ این متود می توان جاهای مناسب را برای آپلود فایل در UI در نظر گرفت.
<br><br>
نمونه پاسخ سرور:
</div>

```json
[
    {
        "id": 258869892,
        "title": "صفحه رو",
        "isRequired": true
    },
    {
        "id": 861089016,
        "title": "صفحه پشت",
        "isRequired": false
    }
]
```

### - Upload Artwork [[currentUser]]
#### آپلود طرح برای چاپ
#### /order/uploadartwork/{orderId}/{artworkSideId}
##### method: post

<div dir="rtl">
برای آپلود طرح برای یک سفارش باید از این متد استفاده کرد. توجه کنید که name فایل در فرم آپلود باید artwork باشد.
</div>

###### orderId:
<div dir=rtl>کد سفارش که در مرحله قبلی توسط سیستم ایجاد شد.</div>

###### artworkSideId:
<div dir=rtl>کد سمت فایل طرح چاپ که با استفاده از متود GetArtworkSides لیستی از آن ها قابل دریافت است.</div>

<div dir=rtl>
در سمت سرور انجام این عملیات باعث ایجاد فایل و همچنین به روز شدن سفارش با اضافه شدن این فایل میشود.
<br><br>
نمونه پاسخ سرور در حالت عملیات موفق:
</div>

```json
{
    "success": true,
    "orderId": 2,
    "fileName": "52343245435_sd6fsd56f7fsf7_front.png",
}
```
<div dir=rtl>
نمونه پاسخ سرور در حالت شکست عملیات:
</div>

```json
{
    "success": false,
    "error": "فایلی آپلود نشده است."
}
```

### - Get shopping cart items list [[currentUser]]
#### لیست آیتم های سبد خرید
#### /order/getshoppingcartitems
##### method: get

<div dir="rtl">
با استفاده از این متود می توان لیستی از تمام سفارش های کاربر را که در وضعیت "سبد خرید" قرار دارند را، دریافت کرد.
این متد از نظر زیرساخت کاملا مشابه متد Get orders list عمل میکند با این تفاوت که نتایجش براساس چاپخانه ها دسته بندی شده است.
</div>

<div dir=rtl>
نمونه پاسخ سرور:
</div>

```json
{
    "success": true,
    "orders": [
        [
            {
                "id": 6,
                "status": "سبد خرید",
                "date": "2017-04-18T10:55:24.108269",
                "pressId": 3,
                "pressName": "البرز",
                "pressDataCost": 80000,
                "pressDataTurnAround": 2,
                "attachments": null,
                "userId": 1,
                "userFullName": "وحید امیری مطلق",
                "pressData": {
                    "serviceId": 2,
                    "serviceTitle": "گلاسه UV"
                }
            }
        ],
        [
            {
                "id": 5,
                "status": "سبد خرید",
                "date": "2017-04-18T10:55:18.5791974",
                "pressId": 4,
                "pressName": "تدبیر",
                "pressDataCost": 50000,
                "pressDataTurnAround": 3,
                "attachments": null,
                "userId": 1,
                "userFullName": "وحید امیری مطلق",
                "pressData": {
                    "serviceId": 2,
                    "serviceTitle": "گلاسه UV"
                }
            },
            {
                "id": 3,
                "status": "سبد خرید",
                "date": "2017-04-18T10:53:30.3792131",
                "pressId": 4,
                "pressName": "تدبیر",
                "pressDataCost": 70000,
                "pressDataTurnAround": 3,
                "attachments": null,
                "userId": 1,
                "userFullName": "وحید امیری مطلق",
                "pressData": {
                    "serviceId": 3,
                    "serviceTitle": "بروشور معمولی"
                }
            }
        ]
    ],
    "allCount": 3
}
```
<div dir=rtl>
نمونه پاسخ سرور در حالت شکست عملیات:
</div>

```json
{
    "success": false,
    "error": "فایلی آپلود نشده است."
}
```

## Press

### - Get full press details by name
#### دریافت اطلاعات کامل چاپخانه با استفاده از نام
#### /presses/getbyname/{pressName}
##### method: get

###### pressName:
<div dir=rtl>اسم چاپخانه مورد نظر</div>
<br>

<div dir=rtl>نمونه پاسخ سرور:</div>

```json
{
    "success": true,
    "press": {
        "id": 4,
        "name": "تدبیر",
        "description": "این مجموعه با بیش از ۱۰ سال سابقه و با استفاده از جدیدترین تجهیزات، آماده خدمت رسانی به سازمان ها و شرکت ها می باشد.",
        "address": "تهران - زرتشت غربی - خ کامبیز - پ ۷",
        "landLine": null,
        "email": "tadbir@gmail.com",
        "logo": null,
        "rate": 2,
        "recommended": false,
        "geoLocation": {
            "latitude": 35.714392058909,
            "longitude": 51.46107673645,
            "zoomLevel": 0,
            "geoLocationType": 1
        },
        "user": null,
        "userId": 0,
        "area": null,
        "averageTurnAround": 2,
        "pressExtraFeatures": [
            {
                "id": 2,
                "title": "ماشین آلات مدرن"
            }
        ],
        "enabledFrom": "0001-01-01T00:00:00"
    }
}
```

### - Get basic press details by id
#### دریافت اطلاعات اصلی چاپخانه با استفاده از id
#### /presses/getbyid/{pressId}
##### method: get

<div dir=rtl>
این متود بیشتر جهت بارگذاری لیست چاپخانه ها قابل استفاده است. به جز پارامتر های ضروری بقیه پارامتر های غیر لازم null هستند.
<br><br>
</div>

###### pressId:
<div dir=rtl>id چاپخانه مورد نظر</div>
<br>

<div dir=rtl>نمونه پاسخ سرور:</div>

```json
{
    "success": true,
    "press": {
        "id": 4,
        "name": "تدبیر",
        "description": "این مجموعه با بیش از ۱۰ سال سابقه و با استفاده از جدیدترین تجهیزات، آماده خدمت رسانی به سازمان ها و شرکت ها می باشد.",
        "address": "تهران - زرتشت غربی - خ کامبیز - پ ۷",
        "landLine": null,
        "email": "tadbir@gmail.com",
        "logo": null,
        "rate": 2,
        "recommended": false,
        "geoLocation": {
            "latitude": 35.714392058909,
            "longitude": 51.46107673645,
            "zoomLevel": 0,
            "geoLocationType": 1
        },
        "user": null,
        "userId": 0,
        "area": null,
        "averageTurnAround": 2,
        "pressExtraFeatures": [
            {
                "id": 2,
                "title": "ماشین آلات مدرن"
            }
        ],
        "enabledFrom": "0001-01-01T00:00:00"
    }
}
```

### Submit comment and rate Press [[currentUser]]
#### ثبت دیدگاه و امتیاز برای چاپخانه
#### /presses/submitcomment
##### method: post
<div dir="rtl">
این متد می تواند برای ثبت دیدگاه و امتیاز برای یک چاپخانه مورد استفاده قرار بگیرد. این متود فقط برای کاربرانی که وارد سیستم شده اند، قابل دسترسی است.
<br><br>
نمونه درخواست به سرور:
</div>



```json
{
    "pressId": 54,
    "text": "Very pleased with the result!",
    "rate": 5
}
```

### Get Press comments list
#### دریافت لیست دیدگاه های چاپخانه
#### /presses/getcomments/{pressId}
##### method: get
<div dir="rtl">
این متد می تواند برای ثبت دیدگاه و امتیاز برای یک چاپخانه مورد استفاده قرار بگیرد. این متود فقط برای کاربرانی که وارد سیستم شده اند، قابل دسترسی است.
<br><br>
</div>

###### pressId:
<div dir=rtl>id چاپخانه مورد نظر</div>

<div dir=rtl>
<br>
نمونه پاسخ سرور:
</div>


```json
[
    {
        "id": 1,
        "text": "کیفیت بالا و قیمت مناسب بود.",
        "rate": 5,
        "userFullName": "وحید امیری مطلق",
        "createdAt": "2017-04-09T13:51:29.6153669"
    },
    {
        "id": 2,
        "text": "متاسفانه زمان تحویل بیشتر از حد معمول بود.",
        "rate": 2,
        "userFullName": "جان دو",
        "createdAt": "2017-04-10T14:30:31.3180278"
    },
    {
        "id": 3,
        "text": "بی کیفیت و گران قیمت",
        "rate": 1,
        "userFullName": "حسین جهانبخش",
        "createdAt": "2017-04-09T14:30:54.8762467"
    }
]
```

## Invoice

### - Generate invoice
#### صدور فاکتور
#### /invoice/generate
##### method: post

<div dir=rtl>
این متود با دریافت یک سری اطلاعات در مورد اضافات سفارش، فاکتور نهایی را صادر یا به روز می کند. پس از صدور فاکتور، آن را بر میگرداند.
<br><br>
نمونه اطلاعات ارسالی:
</div>

```json
{
    "pressCheck": false,
    "designCheck": true,
    "deliveryCheck": false,
    "deliveryType": "ایچاپ",
    "inputAddress": "تهران - زرتشت غربی - خ کامبیز (نوری) پ ۱۶",
    "orderIds": [1,2]
}
```

<br>

```json

```
