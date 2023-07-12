# pyhton-logging-tutorial
سلام خدمت دوستان عزیز 🌹

### آموزش ماژول logging

چند وقت پیش داشتم یکی از پروژه های بزرگمو بهینه سازی و آپدیت میکردم، از اونجایی که این پروژه رو طی یه تایم خیلی فشرده نوشته بودم خیلی جاها برای مطمئن شدن یا لاگ گرفتن از 

```python
print()
```

استفاده کرده بودم.

برای مثال :

```python
print(f"Data Sent to Kafka → {data}")
```

حالا چون مبحثمون لاگ گرفتن هستش دیگه به بخش زیادی از کد که فانکشنال نوشته شده بود اشاره ای نمیکنم 😁 بگذریم.

با خودم گفتم خب الان من هم در محیط تست و توسعه به این پرینت ها نیاز دارم و هم در محیط اجرایی بهشون نیاز ندارم، چیکار میشه کرد ؟

تو همون سرچ اول و خیلی راحت ماژول **logging** رو پیدا کردم.

این ماژول صرفا برای همین کار ساخته شده تا مارو از شر پرینت های زیاد راحت کنه و قابلیت های فوق العاده ای داره.

از اونجایی که مثل اکثر ماژول های کاربری توی منابع فارسی آموزش کاملی براش نبود تصمیم گرفتم این ریپو رو برای توضیح قسمت زیادی از این ماژول که کارمونو صد در صد توی پروژه راه میندازه ایجاد کنم.

---

اول کار میایم یه استفاده عادی ازش میکنیم و بدون هیچ کانفیگ خاصی عملکردش رو نگاه میکنیم :

```python
import logging

logging.debug("Debuging Message...")
logging.info("Info Message...")
logging.warning("Warning Message...")
logging.error("Error Message...")
logging.critical("Critical Message...")
```

خب حالا نگاهی به خروجی کد میندازیم و بعد توضیحات رو شروع میکنیم.

```python
DEBUG:root:Debuging Message...
INFO:root:Info Message...
WARNING:root:Warning Message...
ERROR:root:Error Message...
CRITICAL:root:Critical Message...
```

بعد از اجرای کد های بالا این خروجی رو توی ترمینالمون میبینیم، حالا شاید بگید اینم که توی ترمینال خروجی داده پس چه فرقی با پرینت داره ؟

اولین فرقی توی این متدشه.

```python
logging.disable()
```

شما فرض کنید یه برنامه هزار خطی دارین که توش تقریبا 50 تا پرینت استفاده کردین، طبیعتا برای حذف پرینت ها با مشکلی که من باهاش روبرو هستم مواجه میشین.

با استفاده از این متد ما تمامی لاگ هایی که با ماژول logging گرفتیم رو غیرفعال میکنیم و اون خطوط دیگه اجرا نمیشن.

---

حالا فرق دومش اینه که شما میتونید این لاگ هارو کاستوم کنید و با مقادیر مورد نظرتون توی یک فایل ذخیره کنید.

دو راه برای کانفیگ کردن هست که یکی سخت و مبتدی و یکی یکم طولانی تر و حرفه ای تره.

اول بیاین انواع سطح لاگ رو بررسی کنیم :

<table><tbody><tr><td>DEBUG</td><td>10</td></tr><tr><td>INFO</td><td>20</td></tr><tr><td>WARNING</td><td>30</td></tr><tr><td>ERROR</td><td>40</td></tr><tr><td>CRITICAL</td><td>50</td></tr></tbody></table>

خب حالا با توجه به این جدول میتونیم ببینیم که سطح لاگ دیباگ دارای کمترین مقدار و سطح کریتیکال بالا ترین مقدار رو داره.

---

بریم و کانفیگ ساده اولیه مون رو شروع کنیم :

```python
import logging

logging.basicConfig(
    filename="Demo.log",
    filemode="a",
    format="%(levelname)s - %(asctime)s - in %(filename)s - line %(lineno)d - %(message)s",
    datefmt="%d-%b-%y %H:%M:%S",
    level=logging.DEBUG
)
```

خب در این کانفیگی که انجام دادیم در نگاه اول مطمئنا همه چیز کاملا راحت و قابل فهمه.

*   filename : این آرگومان به آدرس فایلی که میخوایم لاگ هارو توش ذخیره کنیم اشاره داره.
*   filemode : این آرگومان به صورت پیشفرض “a” هستش که یعنی مقادیر جدید رو به مقادیر قبلی اضافه میکنه ولی شما میتونید از انواع مود های دیگه هم استفاده بکنید برای مثال در هنگام استفاده از مود “w” هر بار که شما برنامتون رو ران میکنید تمام لاگ های قبلی پاک میشن.
*   format : این آرگومان فرمت مسیج ذخیره شده هستش که میتونید انواع مقادیری رو که میتونید توی مسیجتون قرار بدید رو از [\>اینجا\<](https://docs.python.org/3/library/logging.html#logrecord-attributes) ببینید.
*   datefmt : اگر در آرگومان format از asctime استفاده کردید در این آرگومان میتونید حتی فرمت ذخیره سازی تایمتون هم وارد کنید. میتونید انواع فرمت های مورد استفاده رو در [\>اینجا\<](https://docs.python.org/3/library/time.html#time.strftime) ببینید.
*   level : این آرگومان به ماژول logging میگه که لطفا بیا و تمام لاگ هایی که بالا تر از مقدار وارد شده هستند رو نمایش بده. برای مثال اگر ما level رو روی logging.ERROR بزاریم برنامه ما دیگه لاگ های در سطح INFO رو ذخیره نمیکنه.

---

من در اوایل استفاده از این ماژول یکی از مشکلام این بود که نمیتونستم توی یه برنامه بزرگ که شامل فولدر ها و ماژول های زیادی میشد تمام لاگ هامو توی یک فایل بریزم. سریع ترین راه حلی که برای این موضوع پیاده سازی کردم کدش اینجا قابل مشاهدست :

```python
import logging
import os

log_dir = os.path.join(__file__.replace("modules/mongo/accounts.py", ""), 'logs')
log_path = os.path.join(log_dir, 'logs.log')

# Create directory if it doesn't exist
os.makedirs(log_dir, exist_ok=True)

logging.basicConfig(
    filename=log_path,
    filemode="a",
    format="%(levelname)s - %(asctime)s - %(message)s",
    datefmt="%d-%b-%y %H:%M:%S"
)
```

حالا در هر فایلی که داشتم در هر کجای برنامه میتونستم تمام لاگ هارو به یک فایل ارسال کنم که محل این فایل با نام logs.log در فولدر logs بود.

---

حالا بریم و یک کم پیشرفته تر یاد بگیریم موضوع رو.

```python
import logging


date_format = "%d-%b-%y %H:%M:%S"
save_format = logging.Formatter(
    "%(levelname)s - %(asctime)s - in %(filename)s - line %(lineno)d - %(message)s",
    datefmt=date_format,
)
file_handler = logging.FileHandler("path/to/file.log")
file_handler.setFormatter(save_format)

logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)
logger.addHandler(file_handler)

logger.debug("DEBUG Message")
```

خب کاری که ما در این بخش انجام دادیم، اومدیم و یک لاگر برای خودمون ساختیم.

کاربرد لاگر ساختن اینه که شما میتونید برای هر یک از سطوح debug, info, error و .. یک لاگر داشته باشید رو هر کدوم رو در یک فایل ذخیره کنید.

با استفاده از تابع `getLogger` من لاگر خودم رو با نام `__name__` ایجاد کردم.

`__name__` همون اسم برناممون هست برای مثال اگر فایل پایتونمون رو به اسم `main.py` ذخیره کنیم متغیر `__name__` ما میشه `main`.

در مرحله بعدی ما اومدیم با استفاده از کلاس `logging.Formatter` همون فرمت هایی که در `logging.basicConfig` استفاده کرده بودیم رو اعمال کردیم.

بعد با استفاده از کلاس `logging.FileHandler` آدرس فایلمون رو مشخص کردیم و فرمت مورد نظرمون رو با استفاده از متود `setFormatter` اعمال کردیم.

و `level` لاگ هامون هم با متود `setLevel`مشخص کردیم و در نهایت تمام این ها رو به لاگرمون با استفاده از متود `addHandler` اضافه کردیم.

---

حالا بیاین فرض کنیم شما یک وب سرویس نوشتید و دیگه فقط زمان و لول و مسیج و … بکارتون نمیاد که توی فایل ذخیره کنید و شما میخواین مثلا آیپی کاربر رو توی لاگ نشون بدید.

برای این کار به شکل زیر عمل میکنیم :

```python
import logging

date_format = "%d-%b-%y %H:%M:%S"
save_format = logging.Formatter("%(asctime)s - %(clientip)s - %(user)s - %(message)s")
file_handler = logging.FileHandler("path/to/file.log")
file_handler.setFormatter(save_format)
logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)
logger.addHandler(file_handler)

data = {"clientip": "192.168.0.1", "user": "ahur4"}
logging.warning("Protocol problem: connection reset", extra=data)
```

در اینجا تنها چیزی که تغییر کرده یکی فرمت ذخیره سازی لاگمون هستش که اومدیم یسری مقادیر کاستوم بهش دادیم.

و یکی هم هنگام لاگ گرفتن هستش که اومدیم و مقدار کاستوم شده رو بهش دادیم که جایگذاری کنه و لاگ مارو برامون کامل کنه دقیقا همونجوری که میخواستیم.

---

آخرین چیزی که در این آموزش یاد میگیریم ساخت level کاستوم و اختصاصی خودمونه.

برای مثال من میخوام جدا از level هایی که بصورت پیشفرض مقدار دهی شدن و قابل استفاده هستن بیام و یک level شخصی بسازم به اسم Ahura با سطح لاگ 15.

برای اینکار به شکل زیر عمل میکنم :

```python
import logging

Ahura = 15
logging.addLevelName(Ahura, "AHURA")

console_handler = logging.StreamHandler()
console_handler.setLevel(Ahura)

date_format = "%Y-%m-%d %H:%M:%S"
formatter = logging.Formatter(
    "%(asctime)s %(levelname)s - %(message)s", datefmt=date_format
)
console_handler.setFormatter(formatter)

logger = logging.getLogger(__name__)
logger.setLevel(Ahura)
logger.addHandler(console_handler)

logger.log(Ahura, "This is a Ahura log message.")
```

که خروجی ما به شکل زیر خواهد بود :

```python
2021-07-01 12:34:56 AHURA - This is a Ahura log message.
```

در این متود `logging.addLevelName` ما سطح و نام level کاستوم خودمون رو وارد کردیم و اون رو به برنامه شناسوندیم.

این کلاس `logging.StreamHandler` معمولا برای تنظیم نوع استریم استفاده میشه که میتونیم برای stdin یا stdout ستش کنیم ولی خب چون ما هدفی برای تنظیم این موارد نداشتیم فقط level کاستوممون رو بهش نشون دادیم.

و چون برای این لاگ اختصاصی مثل سظح لاگ های دیگه متود اختصاصی نداریم از متود `logger.log` استفاده میکنیم و level و پیام خودمون رو بهش به عنوان ورودی میدیم.

این هم از آموزش ماژول logging 😁

آیدی تلگرام بنده : [https://t.me/Ahur4](https://t.me/Ahur4)

آیدی کانال تلگرام : [https://t.me/Ahur4_Javid](https://t.me/Ahur4_Javid)
