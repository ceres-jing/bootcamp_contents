version: '3'

services:
  postgres:
    image: postgres:latest
    environment:
      POSTGRES_DB: your_database_name
      POSTGRES_USER: your_username
      POSTGRES_PASSWORD: your_password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  python:
    image: python:3.9
    volumes:
      - ./import_data.py:/app/import_data.py
      - ./your_file.xlsx:/app/your_file.xlsx
    depends_on:
      - postgres
    command: >
      sh -c "
      pip install pandas psycopg2 &&
      python /app/import_data.py
      "
    environment:
      - DB_HOST=postgres
      - DB_NAME=your_database_name
      - DB_USER=your_username
      - DB_PASS=your_password

volumes:
  postgres_data:
