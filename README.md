# Тестовое задание на позицию DevOps-инженер (Яндекс Браузер для организаций)

- Орлова Светлана
- [Резюме](https://drive.google.com/file/d/1yOBlVtS8oZebL31J7TNquRGH7dTKFSR4/view)

## Задание A1: "Собери и запусти" (Docker, Linux, CLI)

1. Для начала была создана директория `A1/test-project` и выполнен переход в нее:

    ```bash
    mkdir -p A1/test-project && cd A1/test-project
    ```

2. После было создано два файла: `Dockerfile` и `index.html`

    ```bash
    touch Dockerfile && touch index.html
    ```

3. В `index.html` было добавлено приветственное сообщение

    ```bash
    echo "Hello from Svetlana! :)" >> index.html
    ```

4. В `Dockerfile` за основу был взят образ `nginx:alpine` и добавлена инструкция копирования файла `index.html` в специальную директорию `/usr/share/nginx/html`, из которой`nginx` берет приветственную страницу

    ![Dockerfile](img/Dockerfile.png)

5. Для сборки образа использовалась команда

    ```bash
    docker build -t my-web-app:latest .
    ```

6. Контейнер был запущен командой

    ```bash
    docker run -d -p 8080:80 my-web-app:latest
    ```

7. По адресу <http://localhost:8080> вышла следующая страница:

    ![hello_page](img/hello_page.png)

8. Также был написан `compose.yml`, который запускал один сервис `web-app`, используя Dockerfile, и открывал порт 8080, который перенаправлялся на 80 порт сервиса.

    ```text
    services:
        web-app:
            build: .
            ports:
            - 8080:80
    ```

9. Запустить `compose.yml` можно командой

    ```bash
    docker compose up -d
    ```

**Как можно доставить твой index.html в контейнер, не пересобирая образ?**

Лучше всего использовать docker volume, который монтирует папку или файл машины хоста к запущенному контейнеру. То есть если бы мы написали такой `compose.yml`:

```text
services:
    web-app:
        image: nginx:alpine
        ports:
            - "8080:80"
        volumes:
            - ./index.html:/usr/share/nginx/html/index.html:ro
```

То наш `index.html` был одним и тем же файлом на хосте и в контейнере, так что все изменения, сделанные в него, отображались бы в контейнере и были бы видны на странице <http://localhost:8080>. Но изменения работают и в обратную сторону, поэтому в строке `./index.html:/usr/share/nginx/html/index.html:ro` есть в конце `:ro`, это права доступа к файлу: `ro` означает `read only` - права только на чтение файла, чтобы приложение в контейнере не могло изменить наш изначальный файл.

## Задание B1: "Простой скрипт-помощник" (Bash)

Выполненое задание находится в файле `B1/clean_old_logs.sh`

## Задание B2: "Маленькая проблема в Git" (Git)
