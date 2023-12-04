# cmp

> مقایسه بایت به بایت دو فایل.
> اطلاعات بیشتر: <https://www.gnu.org/software/diffutils/manual/html_node/Invoking-cmp.html>.

- نمایش کارکتر و خطی که اولین تفاوت دو فایل در آن یافت شد:

`cmp {{مسیر/به/فایل_اول}} {{مسیر/به/فایل_دوم}}`

- نمایش اطلاعات اولین تفاوت پیدا شده: کاراکتر، شماره خط، بایت ها، و مقادیر آنها:

`cmp --print-bytes {{مسیر/به/فایل_اول}} {{مسیر/به/فایل_دوم}}`

- نمایش شماره بایتها و مقادیر تمامی تفاوت ها:

`cmp --verbose {{مسیر/به/فایل_اول}} {{مسیر/به/فایل_دوم}}`

- مقایسه فایلها در حالت خاموش، تنها مقدار خروجی برنامه در ترمینال در دسترس است:

`cmp --quiet {{مسیر/به/فایل_اول}} {{مسیر/به/فایل_دوم}}`