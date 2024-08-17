# partAuthenticationInterface

سرویس تصدیق اصالت برای بررسی هویت کاربران سامانه و سیستم ها استفاده می شود. 
مجموعه‌ای از اطلاعات (فیلدها) که در تأیید هویت یک کاربر استفاده می‌شود، ترکیب اصالت‌سنجی می گویند. 
سرویس تصدیق اصالت هویت کاربران را بر اساس تنظیماتی که سامانه در سرویس انجام داده بررسی می کند، بدین منظور هر سیستمی که خواهان استفاده از خدمات سرویس تصدیق اصالت است، ابتدا باید نام کاربری و کلمه ی عبور خود را از سرویس دریافت نماید.

منظور از نحوه ی احراز هویت، مشخص کردن پارامترهایی هست که کاربران با آنها قابل شناسایی هستند، است.  هر یک از سامانه‌های عضو می‌توانند انواع مختلفی از ترکیب‌های اصالت‌سنجی را داشته باشند. به عنوان نمونه نام کاربری و کلمه ی عبور یا شماره همراه و رمز عبور و ...

همچنین این امکان برای سرویس ها مهیا شده است که بتوانند اصالت کاربر در سرویس های دیگر را احراز کنند. برای این منظور کافی است دسترسی احراز هویت سرویس مورد نظر را از سرویس تصدیق اطلاعات بگیرند.

اینترفیس حاضر برای ارتباط سیستم ها و سرویس تصدیق اصالت استفاده می شود. اطلاعات شناسایی سیستم که از سرویس تصدیق اصالت دریافت شده در تنظیمات قرار می گیرد.


## الزامات (Requirements)

سیستم برای اینکه بتواند کاربران خود یا سیستم دیگر را اصالت‌سنجی کند، ابتدا باید نام کاربری و کلمه عبور را از سامانه تصدیق اصالت دریافت کند، مانند نمونه زیر:

```javascript
{
  "user": "yourUsername",
  "pass": "yourPassword"
}
```
## نحوه نصب (Installation)

#### ایجاد نمونه 

```javascript
let PartAuthenticationInterface = require("partAuthenticationInterface");
let Authentication = new PartAuthenticationInterface(AuthenticationConfig.global);
let authentication = new Authentication(AuthenticationConfig.instance);
```

## پیکربندی (Configuration)

- برای config این ماژول، باید user و pass ای را که در قسمت قبل گرفتیم در قسمت instance > auth بگزاریم.

- در واسط از ماژول partLogger استفاده می شود. برای تنظیمات، از داکیومنت مربوط به آن استفاده کنید.
```javascript
let partLoggerConfig = {
  global: {
    //partMongoInterfaceConfig: partMongoInterfaceConfig,
    //partDataLayerInterfaceConfig: partDataLayerInterfaceConfig
  },
  instance: {
    sourceTypeWidth: 8,
    sourceNameWidth: 20,
    winstonConfig: {
      handleExceptions: true,
      json: true,
      colorize: true,
      timestamp: function () {
        return (new Date()).toLocaleTimeString();
      },
      prettyPrint: true
    },
    storageConfig: {
      dls: {
        enabled: false,
        storageName: 'Logger@6-test'
      },
      mongo: {
        enabled: false,
        storageName: 'Logger@6-test'
      },
      fileSystem: {
        enabled: false,
        path: 'message.json'
      },
      http: {
        enabled: false,
        host: '127.0.0.1',
        port: '80',
        path: '/service/logServer/saveLog',
        method: 'POST'
      }
    },
    levelConfig: {
      event: {
        view: true,
        save: true,
        color: 'green',
        viewPath: false,
        priority: 2
      },
      warning: {
        view: true,
        save: true,
        color: 'yellowBg',
        viewPath: true,
        priority: 1
      },
      error: {
        view: true,
        save: false,
        color: 'redBg',
        viewPath: true,
        priority: 0
      },
      info: {
        view: true,
        save: true,
        color: 'blueBg',
        viewPath: true,
        priority: 3
      },
      saves: {
        view: true,
        save: true,
        color: 'cyanBg',
        viewPath: true,
        priority: 4
      },
      mosifa: {
        view: true,
        save: false,
        color: 'cyanBg',
        viewPath: true,
        priority: 5
      },
      part: {
        view: true,
        save: true,
        color: 'cyanBg',
        viewPath: true,
        priority: 6
      }
    }
  }
};
let tracerConfig = {
  TraceTitle: 'partAuthenticationInterface', // شخصی سازی شود
  sampler: {
    type: 'probabilistic',
    param: 1
  },
  templates: {
    tagTemplate: {component: 'partAuthenticationInterface'}, // شخصی سازی شود
    logTemplate: {component: 'partAuthenticationInterface'} // شخصی سازی شود
  }
};
```

### با گیت‌وی
```javascript
let AuthenticationConfig = {
    global: {
      gatewayEnable: true,
      host: 'authentication.apipart.ir',
      protocol: 'http',
      port: 80,
      tracerConfig: tracerConfig, // اختیاری است و در صورت عدم ارسال از تریسر پیش فرض استفاده خواهد شد
      partLoggerConfig: partLoggerConfig, // اختیاری است و در صورت عدم ارسال از لاگر پیش فرض استفاده خواهد شد
    },
    instance: {
      auth: {
        user: 'yourUsername', // یوزر و گسوردی که از سامانه تصدیق اصالت دریافت شده است
        pass: 'yourPassword'
      }
    }
  };
```
### بدون گیت‌وی
```javascript
let AuthenticationConfig = {
    global: {
      gatewayEnable: false,
      host: 'authentication.partdp.ir',
      protocol: 'http',
      port: 80,
      tracerConfig: tracerConfig, // اختیاری است و در صورت عدم ارسال از تریسر پیش فرض استفاده خواهد شد
      partLoggerConfig: partLoggerConfig, // اختیاری است و در صورت عدم ارسال از لاگر پیش فرض استفاده خواهد شد
    },
    instance: {
      auth: {
        user: 'yourUsername', // یوزر و گسوردی که از سامانه تصدیق اصالت دریافت شده است
        pass: 'yourPassword'
      }
    }
  };
```


## نحوه استفاده (Usage)
اطلاعات این بخش در فایل [API.md](./API.md) موجود است 

## خطاها و سوالات (Troubleshooting & FAQ)

این بخش اختیاری است. در صورت وجود، مشکلات رایجی را که افراد با آن روبرو می‌شوند به همراه راه حل‌ها، بیان کنید.
