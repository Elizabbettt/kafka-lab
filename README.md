# Kafka Lab — Контроль домашних заданий в школе

Лабораторная работа по потоковой обработке данных в реальном времени с использованием Apache Kafka.

## Описание

Реализован полный цикл работы с сообщениями в Kafka на примере **школьной системы контроля домашних заданий**. Producer генерирует события о домашних работах (ученик, учитель, предмет, задание, даты, оценка, комментарий) и отправляет их в топик. Consumer читает сообщения из топика, валидирует их и выводит результат в консоль.

Около 20% сообщений намеренно генерируются с невалидными данными (оценка ниже 2 или выше 5) для демонстрации работы валидации.

## Стек технологий

- **Apache Kafka 4.2.0** (KRaft mode, без ZooKeeper) — брокер сообщений
- **Python 3** — язык разработки скриптов
- **kafka-python** — клиентская библиотека для работы с Kafka из Python

## Структура проекта
kafka-lab/
├── generator.py # Генерация случайных сообщений (вне Producer — принцип SOLID)
├── producer.py # Отправка сообщений в Kafka-топик
├── consumer.py # Чтение и валидация сообщений
├── requirements.txt # Зависимости Python
├── README.md # Описание проекта
└── Screenshot.png # Скриншот работы программы


## Формат сообщения

```json
{
  "student": "Иванов Иван",
  "teacher": "Петрова Мария Ивановна",
  "subject": "Математика",
  "homework": "§15, №123-135",
  "date_assigned": "2026-03-20",
  "date_submitted": "2026-03-22",
  "grade": 4,
  "comment": "Хорошо, но есть ошибки в вычислениях"
}

## Правила валидации (Consumer)


Поле	Условие невалидности
grade	< 2 или > 5
subject	Не входит в список: Математика, Русский язык, Физика, Информатика, История, Английский язык
student	Отсутствует или пустое
teacher	Отсутствует или пустое
homework	Отсутствует или пустое
date_assigned	Отсутствует или не в формате YYYY-MM-DD
date_submitted	Отсутствует или не в формате YYYY-MM-DD

## Запуск


Требования
Java 17+

Python 3.8+

Apache Kafka 4.2.0

1. Установить зависимости Python
bash
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
2. Запустить Kafka (окно 1)
bash
cd C:\kafka_2.13-4.2.0
bin\windows\kafka-server-start.bat config\server.properties
3. Создать топик (окно 2)
bash
bin\windows\kafka-topics.bat --create --topic homework-control --bootstrap-server localhost:9092
4. Запустить Consumer (окно 3)
bash
cd C:\Users\dns\Desktop\kafka_lab
venv\Scripts\activate
python consumer.py
5. Запустить Producer (окно 4)
bash
cd C:\Users\dns\Desktop\kafka_lab
venv\Scripts\activate
python producer.py
Пример вывода
Producer:

text
📤 SENT: {"student": "Иванов Иван", "teacher": "Петрова Мария Ивановна", "subject": "Математика", "grade": 4}
Consumer:

text
📥 RECEIVED
✓ VALID MESSAGE: {"student": "Иванов Иван", "grade": 4}

📥 RECEIVED
✗ NOT VALID
  - Оценка вне допустимого диапазона (2-5): 6

## Результат работы

На скриншоте видно, что сообщения, отправленные Producer, успешно доставляются Consumer.

Автор
Студентка [Михайлова Елизавета]
Группа [ИТБ-23]

22.03.2026
