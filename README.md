import pandas as pd
import psycopg2
import os

DB_HOST = os.environ.get('DB_HOST')
DB_NAME = os.environ.get('DB_NAME')
DB_USER = os.environ.get('DB_USER')
DB_PASS = os.environ.get('DB_PASS')

EXCEL_FILE_PATH = '/app/your_file.xlsx'

df = pd.read_excel(EXCEL_FILE_PATH)

conn = psycopg2.connect(
    host=DB_HOST,
    database=DB_NAME,
    user=DB_USER,
    password=DB_PASS
)
cursor = conn.cursor()

columns = ','.join(df.columns)
create_table_query = f"CREATE TABLE your_table_name ({columns} TEXT);"
cursor.execute(create_table_query)
conn.commit()

for row in df.itertuples(index=False):
    placeholders = ','.join(['%s'] * len(row))
    insert_query = f"INSERT INTO your_table_name ({columns}) VALUES ({placeholders});"
    cursor.execute(insert_query, row)
    conn.commit()

cursor.close()
conn.close()

print("Data imported successfully!")
