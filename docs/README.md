# 🤖 RGB — Электронная стенгазета класса робототехники

Интерактивная стенгазета с разделами:
- 🚀 Наши проекты
- 🏆 Гений недели
- 📅 Календарь событий
- 😄 Робо-юмор
- 🧩 Головоломка
- 🔧 Совет от мастера
- 📸 Фото недели

## 🌐 Как посмотреть

Откройте: **https://ваш-username.github.io/robot-gazeta/**

## 🔐 Режим администратора

1. Нажмите **«🔐 Войти как админ»** в правом верхнем углу.
2. Введите пароль (по умолчанию `rgb2025`).
3. Теперь вы можете:
   - Редактировать текстовые блоки (кнопка «✏️ Редактировать»).
   - Загружать «Фото недели».
   - Вставлять текст без стилей — при копировании из других сайтов стили автоматически удаляются.

## ⚙️ Настройка

Для работы фото и редактирования нужен **Supabase**.

### 1. Создайте проект в Supabase
Зарегистрируйтесь на [supabase.com](https://supabase.com), создайте новый проект.

### 2. Создайте bucket для фото
- Storage → New bucket → Name: `photos` → **Public bucket** ✅ → Save.

### 3. Создайте таблицу контента
SQL Editor → выполните:

```sql
create table if not exists public.content (
  id text primary key,
  html text not null,
  updated_at timestamptz default now()
);

alter table public.content enable row level security;

create policy "Public read content"   on public.content for select to public using (true);
create policy "Public insert content" on public.content for insert to public with check (true);
create policy "Public update content" on public.content for update to public using (true);

create policy "Public Read for photos"   on storage.objects for select to public using (bucket_id = 'photos');
create policy "Public Insert for photos" on storage.objects for insert to public with check (bucket_id = 'photos');
create policy "Public Update for photos" on storage.objects for update to public using (bucket_id = 'photos');
create policy "Public Delete for photos" on storage.objects for delete to public using (bucket_id = 'photos');