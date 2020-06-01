# Django Next 템플릿

## 프로젝트 초기 설정

**1. Next.js 초기화**

```sh
$ npx create-next-app client
```

만약 Next 대신 create-react-app으로 프로젝트를 생성한다면 `tty: true`옵션과 `stdin_open: true`옵션을 포함할 것.

```
client:
    build:
      context: .
      dockerfile: ./docker/DockerFile.client
    ports:
      - 3000:3000
    stdin_open: true
    tty: true
    volumes:
      - ./client:/app
    command: /bin/bash -c "npm run dev"
```

**2. Docker 이미지 빌드**

```sh
$ docker-compose build
```

**3. Django 프로젝트 초기화**

```sh
$ docker-compose run server django-admin startproject .
```

**4. Django 앱 초기화**

```sh
$ docker-compose run server python3 manage.py startapp {앱 이름}
```

## 프로젝트 Hot reload

**1. Docker 실행**

```sh
$ docker-compose up [-d]
```

**2. Django 모델 mgirate(모델 변경 시 마다 실행)**

`run`으로 migrate해도 되나, `exec`를 사용하면 기존 컨테이너를 사용할 수 있으므로 개발단계에서는 `exec`로 개발

```sh
$ docker-compose exec {프로젝트 폴더명}_server_1 ./manage.py makemigrations # migration 파일 생성
$ docker-compose exec {프로젝트 폴더명}_server_1 ./manage.py sqlmigrate {앱 이름} # sql code 확인
$ docker-compose exec {프로젝트 폴더명}_server_1 ./manage.py migrate # db migrate
```

**3. (이미 띄워진) Docker 서비스 실행/중단**

```
$ docker-compose [start|stop]
```

**4. (이미 띄워진) Docker 서비스 삭제**

```sh
$ docker-compose down [--volume] # --volume 옵션 시 volume도 함께 삭제
```
