# 🦊 Firefox in Podman Container (Fedora 43 + Wayland)

Этот README описывает пошаговую настройку контейнера Fedora 43 с браузером Firefox, работающим под Wayland.

---

## 📦 Создание контейнера

Удаляем старый контейнер (если был):

```bash
podman rm -f firefox-fedora
```

Создаём новый контейнер Fedora 43 с пробросом Wayland:

```bash
podman run -it --name firefox-fedora \
  --network=host \
  --security-opt label=disable \
  --security-opt seccomp=unconfined \
  -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
  -e XDG_RUNTIME_DIR=/tmp/runtime-host \
  -v $XDG_RUNTIME_DIR:/tmp/runtime-host \
  fedora:43 bash
```

---

## 🔧 Настройка внутри контейнера

Обновляем систему:

```bash
dnf upgrade --refresh -y
```

Устанавливаем Firefox:

```bash
dnf install -y firefox
```

---

## 🚀 Запуск Firefox

Внутри контейнера:

```bash
firefox
```

Из хоста:

```bash
podman exec -it firefox-fedora firefox
```

---

## ⚠️ Важные моменты

- `--network=host` обеспечивает доступ к сети без таймаутов.  
- `--security-opt label=disable` и `--security-opt seccomp=unconfined` снимают ограничения SELinux и seccomp, чтобы Firefox мог подключиться к Wayland.  
- Переменные `WAYLAND_DISPLAY` и `XDG_RUNTIME_DIR` пробрасывают сокет Wayland внутрь контейнера.  

---

## ✅ Итог

Теперь у вас есть контейнер Fedora 43 с установленным Firefox, который запускается под Wayland. Этот подход сохраняет изоляцию Podman, но позволяет использовать графический браузер.
```
аналогично оформить README.md для контейнера с Brave через Flatpak, чтобы у тебя были оба варианта в репозитории?
