# دموی OpenMAIC-fa روی maic.greenlanco.ir

این پوشه یک دموی عمومی از OpenMAIC-fa را پشت nginx سایت greenlanco.ir بالا می‌آورد.
مدل زبانی از OpenRouter است و ورود به دمو با کد دسترسی (`ACCESS_CODE`) بسته شده است.

## راه‌اندازی روی سرور

```bash
git clone https://github.com/bijanshdevops/OpenMAIC-fa.git
cd OpenMAIC-fa
cp demo/demo.env.example demo/demo.env
nano demo/demo.env   # OPENROUTER_API_KEY و ACCESS_CODE را پر کنید
docker compose -p openmaic-demo -f demo/compose.demo.yml --env-file demo/demo.env up -d --build
```

- ساخت تصویر اول چند دقیقه طول می‌کشد.
- کانتینر با نام مستعار `openmaic-demo-app` روی شبکهٔ `greenlanco_internal` قرار می‌گیرد و nginx سایت درخواست‌های `maic.greenlanco.ir` را به آن می‌فرستد.
- برای بررسی روی خود سرور: `curl -I http://127.0.0.1:3500/api/health`

## به‌روزرسانی

```bash
git pull
docker compose -p openmaic-demo -f demo/compose.demo.yml --env-file demo/demo.env up -d --build
```

## نکته‌ها

- `demo/demo.env` کلید OpenRouter را دارد؛ آن را commit نکنید (در `.gitignore` هست).
- برای کلید OpenRouter سقف هزینهٔ ماهانه بگذارید؛ هر کسی که کد دسترسی را داشته باشد از این کلید استفاده می‌کند.
- کلاس‌ها و تنظیمات بازدیدکنندگان در مرورگر خودشان ذخیره می‌شود؛ پوشهٔ `/app/data` در volume `data` می‌ماند.
- توقف: `docker compose -p openmaic-demo -f demo/compose.demo.yml --env-file demo/demo.env down`
