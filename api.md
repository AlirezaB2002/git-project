### فهرست 

1. [تصدیق اصالت کاربر (‌authenticate)](#authenticate)
1. [درج کاربر (addUser)](#addUser)
1. [دریافت ترکیب‌های اصالت‌سنجی (getAllUsers)](#getAllUsers)
1. [دریافت کاربران (getUser)](#getUser)
1. [حذف کاربر (removeUser)](#removeUser)
1. [تنظیم وضعیت یک کاربر (setStatus)](#setStatus)
1. [ویرایش یک کاربر (editUser)](#editUser)
1. [دریافت توکن یکبار مصرف (addOneTimeToken)](#addOneTimeToken)
1. [احراز هویت توکن یکبار مصرف (authenticateOneTimeToken)](#authenticateOneTimeToken)

#### مقدمه ####
هر سیستمی که خواهان استفاده از سرویس تصدیق اصالت است، ابتدا باید نام کاربری و کلمه ی عبور خود را از سرویس تصدیق اصالت دریافت کند. همچنین در کلیه ی متدهایی که معرفی می شود، چنانچه system ارسال نشود تصدیق اصالت کاربر در سامانه درخواست دهنده و چنانچه system ارسال شود، تصدیق اصالت در سامانه داده شده انجام می شود.

> شایان ذکر است که چنانچه سیستمی قصد تصدیق اصالت کاربر در سیستم دیگر را دارد، ابتدا باید دسترسی این امکان را دریافت کرده باشد.

### تصدیق اصالت کاربر (‌authenticate) <a name="authenticate"></a>

- سامانه‌ها به‌منظور تصدیق‌اصالت کاربران خود یا دیگر سامانه‌ها، از‌این متد استفاده می‌کنند.

- برای تصدیق اصالت یک کاربر، باید تمام فیلدهای یکی از انواع ترکیب های اصالت سنجی ارسال شود. بدین منظور پارامترهای ترکیب اصالت (به عنوان نمونه نام کاربری و کلمه عبور) کاربر برای احراز‌هویت ارسال می‌شود.


**نحوه فراخوانی:**

```javascript
ai.authenticate(fields, system, trackingHeaders)
     .then(function () {
        //code here
      })
     .fail(function () {
        //code here
      });
```

**پارامترهای ورودی:**
```javascript
/**
 * @param {!object} fields فیلدهای لازم برای اصالت سنجی
 * @[param {!string} [system نام سامانه
 * @param trackingHeaders {object} آبجکت توصیفی برای این درخواست
 * @param {!string} trackingHeaders.request-id توکن یونیک
 * @param {!string} trackingHeaders.userIp آی پی درخواست دهنده
 */

fields : {username: 'test1',password: 'test'}
system : 'test'
trackingHeaders : {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {
    "entryId": "29503126-c1c1-43bf-a3d8-099d0f3adc47",
    "status": "active",
    "uniqueFields": [
      {
        "username": "test_0.06437536170148817"
      }
    ],
 'userId':''
  },
  "meta": {
    "shamsiDate": 13980413083008352
  }
}
```
#### حالت خطا 
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413085936852
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "should be object",
        "path": "/fields"
      }
    ]
  }
}
```
```javascript
{
  "meta": {
    "code": "invalidAuthEntry",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413085936950
  },
  "data": {
    "message": {
      "fa": "ساختار ترکیب اصالت سنجی معتبر نمی باشد!",
      "en": "Invalid authentication entry structure!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "authEntryNotFound",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413085937128
  },
  "data": {
    "message": {
      "fa": "ترکیب اصالت سنجی یافت نشد!",
      "en": "Authentication entry not found!"
    }
  }
}
```


### درج کاربر (addUser) <a name="addUser"></a>

- برای اضافه کردن کاربر به سیستم از این متد استفاده می شود.

**نحوه فراخوانی:**

```javascript
ai.addUser(fields, status, system, trackingHeaders)
    .then(function () {
       //code here
     })
    .fail(function () {
       //code here
     });
```

**پارامترهای ورودی:**

```javascript
/**
 * @param {!object} fields  فیلدهای لازم برای ساخت ترکیب اصالت سنجی؛ مثال: username, password
 * @param {!string} [status]  وضعیت سامانه
 * @param {!string} [system]  نام سامانه
 * @param trackingHeaders هدر
 */

fields : { username : 'test' , password: 'test'}
status = 'active'
system = 'test'
trackingHeaders  : {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {
    "fields": {
      "username": "test_0.26215693365430726",
      "password": "Hashed Data..."
    },
    "status": "active",
    "system": "test",
    "entryId": "88932468-aa82-4fc0-bb90-bf09d8cebd8c"
  },
  "meta": {
    "shamsiDate": 13980413083158570
  }
}
```
#### حالت خطا 
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413090840288
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "should be object",
        "path": "/fields"
      }
    ]
  }
}
```
```javascript
{
  "meta": {
    "code": "invalidAuthEntry",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413090840372
  },
  "data": {
    "message": {
      "fa": "ساختار ترکیب اصالت سنجی معتبر نمی باشد!",
      "en": "Invalid authentication entry structure!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413090840392
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "وضعیت معتبر نمی‌باشد!",
        "path": "/status"
      }
    ]
  }
}
```
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413090840428
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "نام سامانه معتبر نمی‌باشد!",
        "path": "/system"
      },
      {
        "message": "نام سامانه معتبر نمی‌باشد!",
        "path": "/system"
      }
    ]
  }
}
```

```javascript
{
  "meta": {
    "code": "systemNotFound",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413090840508
  },
  "data": {
    "message": {
      "fa": "سامانه مورد نظر یافت نشد!",
      "en": "System not found!"
    }
  }
}
```


### دریافت ترکیب‌های اصالت‌سنجی (getAllUsers) <a name="getAllUsers"></a>
- برای دریافت لیست کاربران یک سازمان از این متد استفاده می شود.

**نحوه فراخوانی:**

```javascript
ai.getAllUsers(system, trackingHeaders
    .then(function () {
       //code here
     })
    .fail(function () {
       //code here
     });
```

**پارامترهای ورودی:**

```javascript
/**
 * @summary دریافت کاربران
       * @description لیستی از کاربران عضو در سامانه تصدیق اصالت را برمی گرداند.
       * @param {!object} trackingHeaders  آبجکت رهگیری
       * @param {!string} trackingHeaders.request-id توکن یونیک برای این درخواست
       * @param {!string} trackingHeaders.part-trace-id توکن یونیک برای این درخواست
       * @param {string}  trackingHeaders.userIp آی پی کاربر
       * @param {!string} [system] سامانه
  */

system : 'test'
trackingHeaders : {request-id: uuid() , userIp: '127.0.0.1'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": [
    {
      "entryId": "8b706589-1a2c-4949-9141-016989ee3a18",
      "system": "test",
      "fields": {
        "password": "Hashed Data...",
        "username": "rezash"
      },
      "status": "active"
    },
    {
      "entryId": "87111dd4-21c2-4710-b638-7f7258f45da4",
      "system": "test",
      "fields": {
        "password": "Hashed Data...",
        "username": "rezashaa"
      },
      "status": "active"
    }
  ],
  "meta": {
    "shamsiDate": "13980413083158212"
  }
}
```
#### حالت خطا 
```javascript
{
  "meta": {
    "code": "systemNotFound",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413091307286
  },
  "data": {
    "message": {
      "fa": "سامانه مورد نظر یافت نشد!",
      "en": "System not found!"
    }
  }
}
```




### دریافت کاربران (getUser) <a name="getUser"></a>

- برای دریافت اطلاعات کاربر از این متد استفاده می شود.

**نحوه فراخوانی:**

```javascript
ai.getUser(entryId, system, fields, trackingHeaders)
    .then(function () {
       //code here
     })
    .fail(function () {
       //code here
     });
```

**پارامترهای ورودی:**
```javascript
/**
   * @param {!string} [system]  نام سامانه
   * @param {string} entryId آی دی کاربر که توسط آن می‌توان اطلاعات کاربر را دریافت کرد
   * @param {string} fields لیست فیلدهای درخواستی
 * @param trackingHeaders {object} آبجکت توصیفی برای این درخواست
 * @param {!string} trackingHeaders.request-id توکن یونیک
 * @param {!string} trackingHeaders.userIp آی پی درخواست دهنده
   */

system : 'test'
entryId : '2'
fields : {}
trackingHeaders : {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {
    "entryId": "88932468-aa82-4fc0-bb90-bf09d8cebd8c",
    "system": "test",
    "fields": {
      "password": "Hashed Data...",
      "username": "test_0.26215693365430726"
    },
    "status": "active"
  },
  "meta": {
    "shamsiDate": "13980413083158624"
  }
}
```
#### حالت خطا 

```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413091831018
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "شناسهٔ مدخل معتبر نمی‌باشد!",
        "path": "/entryId"
      }
    ]
  }
}
```




### حذف کاربر (removeUser) <a name="removeUser"></a>
- سامانه‌ها به‌منظور حذف کاربر خود یا دیگر سامانه‌ها، از متد زیر استفاده می‌کنند.

**نحوه فراخوانی:**

```javascript
ai.removeUser(uniqueFields, system, trackingHeaders)
    .then(function () {
       //code here
     })
    .fail(function () {
       //code here
     });
```

**پارامترهای ورودی:**

```javascript
/**
 * @param {!object} uniqueFields فیلد(های) یکتاساز؛ مثال: username
 * @param {!string} [system] نام سامانه
 * @param trackingHeaders {object} آبجکت توصیفی برای این درخواست
 * @param {!string} trackingHeaders.request-id توکن یونیک
 * @param {!string} trackingHeaders.userIp آی پی درخواست دهنده
 */

uniqueFields : {username: 'test'}
system : 'test'
trackingHeaders : {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {},
  "meta": {
    "shamsiDate": 13980413083158842
  }
}
```
#### حالت خطا 

```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413091954176
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "نام سامانه معتبر نمی‌باشد!",
        "path": "/system"
      },
      {
        "message": "نام سامانه معتبر نمی‌باشد!",
        "path": "/system"
      }
    ]
  }
}
```
```javascript
{
  "meta": {
    "code": "invalidAuthEntry",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413091954284
  },
  "data": {
    "message": {
      "fa": "ساختار ترکیب اصالت سنجی معتبر نمی باشد!",
      "en": "Invalid authentication entry structure!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "authEntryNotFound",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413091954368
  },
  "data": {
    "message": {
      "fa": "ترکیب اصالت سنجی یافت نشد!",
      "en": "Authentication entry not found!"
    }
  }
}
```




### تنظیم وضعیت یک کاربر (setStatus) <a name="setStatus"></a>
- سامانه‌ها به‌منظور تنظیم وضعیت کاربران خود یا دیگر سامانه‌ها در سرویس تصدیق اصالت، از‌این تابع استفاده می‌کنند.

> این تابع، نام‌کاربری، وضعیت و نام سامانه را گرفته و برای تنظیم وضعیت کاربر، به سامانه‌ی تصدیق‌اصالت ارسال می‌کند.

**نحوه فراخوانی:**

```javascript
ai.setStatus(uniqueFields, status, system, trackingHeaders)
     .then(function () {
        //code here
      })
     .fail(function () {
        //code here
      });
```

**پارامترهای ورودی:**

```javascript
/**
 * @param {!object} uniqueFields فیلد(های) یکتاساز؛ مثال: username
 * @param {!string} status وضعیت سامانه
 * @param {!string} [system] نام سامانه
 * @param trackingHeaders {object} آبجکت توصیفی برای این درخواست
 * @param {!string} trackingHeaders.request-id توکن یونیک
 * @param {!string} trackingHeaders.userIp آی پی درخواست دهنده
 */

uniqueFields : {'username': 'test'  }
status : 'active'
system : 'test'
trackingHeaders : {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {},
  "meta": {
    "shamsiDate": 13980413084230590
  }
}
```
#### حالت خطا 

```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413092642180
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "should be object",
        "path": "/uniqueFields"
      },
      {
        "message": "وضعیت معتبر نمی‌باشد!",
        "path": "/status"
      }
    ]
  }
}
```
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413092642216
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "should be object",
        "path": "/uniqueFields"
      }
    ]
  }
}
```
```javascript
{
  "meta": {
    "code": "invalidAuthEntry",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413092642282
  },
  "data": {
    "message": {
      "fa": "ساختار ترکیب اصالت سنجی معتبر نمی باشد!",
      "en": "Invalid authentication entry structure!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "authEntryNotFound",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413092642464
  },
  "data": {
    "message": {
      "fa": "ترکیب اصالت سنجی یافت نشد!",
      "en": "Authentication entry not found!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413092642492
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "نام سامانه معتبر نمی‌باشد!",
        "path": "/system"
      },
      {
        "message": "نام سامانه معتبر نمی‌باشد!",
        "path": "/system"
      }
    ]
  }
}
```




### ویرایش یک کاربر (editUser) <a name="editUser"></a>
- سامانه‌ها به‌منظور ویرایش ترکیب اصالت‌سنجی کاربران خود یا دیگر سامانه‌ها، از متد زیر استفاده می‌کنند.

> اگر سامانه‌ای چندین نوع ترکیب داشته باشد، هنگام ویرایش یک ترکیب، می‌تواند فیلدهایی که از قبل وجود نداشته‌اند را نیز مقداردهی کند به‌شرط‌ آنکه آن فیلدها در انواع دیگر ترکیب‌های تعریف شده توسط آن سامانه موجود باشند.

> نام‌کاربری و نام‌سامانه را به‌همراه مقادیر جدید اطلاعات کاربر گرفته و برای ویرایش به سامانه‌ی تصدیق‌اصالت ارسال می‌کند.

> اگر سامانه‌ای قصد ویرایش کاربر خود را دارد، نیازی به ارسال نام سامانه ندارد

**نحوه فراخوانی:**

```javascript
ai.editUser(uniqueFields, newValues, system, trackingHeaders)
     .then(function () {
        //code here
      })
     .fail(function () {
        //code here
      });
```
**پارامترهای ورودی:**

```javascript


/**
 * @param {!object} uniqueFields فیلد(های) یکتاساز؛ مثال: username
 * @param {!object} newValues مقادیر جدید اطلاعات کاربر
 * @param {!string} [system] نام سامانه
 * @param trackingHeaders {object} آبجکت توصیفی برای این درخواست
 * @param {!string} trackingHeaders.request-id توکن یونیک
 * @param {!string} trackingHeaders.userIp آی پی درخواست دهنده
 */
uniqueFields : { 'username': 'testedit3'}
newValues : {  'username': 'test3Edited','password': 'reza' }
system : 'test'
trackingHeaders : {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {
    "count": 1
  },
  "meta": {
    "shamsiDate": 13980413084329116
  }
}
```
#### حالت خطا 
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413092956782
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "should be object",
        "path": "/uniqueFields"
      },
      {
        "message": "should be object",
        "path": "/newValues"
      }
    ]
  }
}
```
```javascript
{
  "meta": {
    "code": "invalidAuthEntry",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413092956860
  },
  "data": {
    "message": {
      "fa": "ساختار ترکیب اصالت سنجی معتبر نمی باشد!",
      "en": "Invalid authentication entry structure!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "authEntryNotFound",
    "sourceType": "service",
    "sourceName": "authentication",
    "version": "7.0.0",
    "shamsiDate": 13980413092956942
  },
  "data": {
    "message": {
      "fa": "ترکیب اصالت سنجی یافت نشد!",
      "en": "Authentication entry not found!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413092956954
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    },
    "additionalInfo": [
      {
        "message": "نام سامانه معتبر نمی‌باشد!",
        "path": "/system"
      }
    ]
  }
}
```



### دریافت توکن یکبار مصرف (addOneTimeToken) <a name="addOneTimeToken"></a>
- برای دریافت توکن یکبار مصرف جهت احراز هویت از این متد استفاده می‌شود. زمان استفاده از این توکن محدود است و منقضی خواهد شد.

**نحوه فراخوانی:**

```javascript
addOneTimeToken(data, system, trackingHeaders)
     .then(function () {
        //code here
      })
     .fail(function () {
        //code here
      });
```

**پارامترهای ورودی:**


```javascript
/**
       * @description دریافت توکن یکبار مصرف برای احراز هویت
       * @memberOf module\partAuthenticationInterface\main.PartAuthenticationGlobalNs.PartAuthenticationInstanceNs
       * @param data {object}
       * @param data.uniqueFields {object} فیلد یا فیلد های یونیک در سامانه مورد نظر مانند username or mobile ...
       * @param data.tokenName {!string} نام محلی که برای دریافت توکن این ای پی آی را صدا زده example: 'login' , 'anything'
       * @param data.addIfNotExist {!boolean} مشخص کننده اینکه در سامانه تصدیق اصالت ثبتنام شود یا خیر
       * @param data.ttl {?number} مدت زمان معتبر بودن توکن دریافتی به ثانیه
       * @param {!object} trackingHeaders کاستوم هدر
       * @param {!object} trackingHeaders.request-id توکن یونیک برای این درخواست
       * @param {!object} trackingHeaders.part-trace-id توکن یونیک برای این درخواست
       * @param {string}  trackingHeaders.userIp آی پی کاربر

 */
       data= {
          uniqueFields: {
            username: username
          },
          tokenName: 'login',
          addIfNotExist: true,
          ttl: 30
        };
        system = 'test'
      trackingHeaders = {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {
    "fields": {
      "username": "test_0.6191361766170163"
    },
    "status": "active",
    "entryId": "203fc32f-9c98-4aeb-96ad-b63956f1b71b",
    "userId": "203fc32f-9c98-4aeb-96ad-b63956f1b71b",
    "token": "47317"
  },
  "meta": {
    "shamsiDate": "13980812082719355"
  }
}
```
```javascript
{
  "data": {
    "token": "47317"
  },
  "meta": {
    "shamsiDate": "13980812082719355"
  }
}
```
#### حالت خطا 

```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413092956782
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "authEntryNotFound",
    "sourceType": "service",
    "sourceName": "partServiceAuthentication",
    "version": "7.2.1",
    "shamsiDate": "13980812082849440"
  },
  "data": {
    "message": {
      "fa": "ترکیب اصالت سنجی یافت نشد!",
      "en": "Authentication entry not found!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "systemNotFound",
    "sourceType": "service",
    "sourceName": "partServiceAuthentication",
    "version": "7.2.1",
    "shamsiDate": "13980812082849440"
  },
  "data": {
    "message": {
      "fa": " سامانه یافت نشد!",
      "en": "system not found!"
    }
  }
}
```





### احراز هویت با توکن یکبار مصرف (authenticateOneTimeToken) <a name="authenticateOneTimeToken"></a>
- جهت احراز هویت کاربر با توکن از این متد استفاده می‌شود. توکن مورد استفاده تنها یکبار قابل استفاده است.

**نحوه فراخوانی:**

```javascript
authenticateOneTimeToken(data, system, trackingHeaders)
     .then(function () {
        //code here
      })
     .fail(function () {
        //code here
      });
```

**پارامترهای ورودی:**


```javascript
/**
       * @description احراز هویت توکن دریافت شده
       * @memberOf module\partAuthenticationInterface\main.PartAuthenticationGlobalNs.PartAuthenticationInstanceNs
       * @param data {object}
       * @param data.uniqueFields {object} فیلد یا فیلد های یونیک در سامانه مورد نظر مانند username or mobile ...
       * @param data.tokenName {!string} نام محلی که برای دریافت توکن این ای پی آی را صدا زده example: 'login' , 'anything'
       * @param data.tokenValue {!string} توکن احراز هویت دریافت شده
       * @param {!object} trackingHeaders کاستوم هدر
       * @param {!object} trackingHeaders.request-id توکن یونیک برای این درخواست
       * @param {!object} trackingHeaders.part-trace-id توکن یونیک برای این درخواست
       * @param {string}  trackingHeaders.userIp آی پی کاربر
 */
       data= {
          uniqueFields: {
            username: username
          },
          tokenName: 'login',
          tokenValue: '47317'
        };
        system = 'test'
      trackingHeaders = {'request-id': 'توکن یونیک', userIp: 'آی پی درخواست دهنده'}
```

**خروجی‌های ممکن:**
#### حالت صحیح

```javascript
{
  "data": {
    "uniqueFields":[ {
      "username": "test_0.6191361766170163"
    }],
    "status": "active",
    "entryId": "203fc32f-9c98-4aeb-96ad-b63956f1b71b",
    "userId": "203fc32f-9c98-4aeb-96ad-b63956f1b71b"
  },
  "meta": {
    "shamsiDate": "13980812082719355"
  }
}
```
#### حالت خطا 

```javascript
{
  "meta": {
    "code": "validationError",
    "sourceType": "module",
    "sourceName": "partFramework",
    "version": "9.0.5",
    "shamsiDate": 13980413092956782
  },
  "data": {
    "message": {
      "fa": "داده‌های ورودی نامعتبرند!",
      "en": "Invalid input data!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "authEntryNotFound",
    "sourceType": "service",
    "sourceName": "partServiceAuthentication",
    "version": "7.2.1",
    "shamsiDate": "13980812082849440"
  },
  "data": {
    "message": {
      "fa": "ترکیب اصالت سنجی یافت نشد!",
      "en": "Authentication entry not found!"
    }
  }
}
```
```javascript
{
  "meta": {
    "code": "systemNotFound",
    "sourceType": "service",
    "sourceName": "partServiceAuthentication",
    "version": "7.2.1",
    "shamsiDate": "13980812082849440"
  },
  "data": {
    "message": {
      "fa": " سامانه یافت نشد!",
      "en": "system not found!"
    }
  }
}
```
