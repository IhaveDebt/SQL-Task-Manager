# tasks.py
import psycopg2

DB_PARAMS = {
    "dbname": "tasks_db",
    "user": "postgres",
    "password": "password",
    "host": "localhost",
    "port": 5432
}

def add_task(title, priority):
    conn = psycopg2.connect(**DB_PARAMS)
    cur = conn.cursor()
    cur.execute("INSERT INTO tasks(title, priority) VALUES (%s, %s);", (title, priority))
    conn.commit()
    cur.close()
    conn.close()
    print(f"Task added: {title}")

def list_tasks():
    conn = psycopg2.connect(**DB_PARAMS)
    cur = conn.cursor()
    cur.execute("SELECT id, title, priority, done FROM tasks ORDER BY priority DESC;")
    rows = cur.fetchall()
    for r in rows:
        print(r)
    cur.close()
    conn.close()

if __name__ == "__main__":
    add_task("Finish project", 5)
    list_tasks()
