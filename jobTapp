import os  # Βιβλιοθήκη της Python για να διαβάζει το σύστημα
import mysql.connector
from flask import Flask, render_template, request, redirect, url_for
from dotenv import load_dotenv  # Φέρνουμε τη βιβλιοθήκη που εγκαταστήσαμε


# Φορτώνει τις μεταβλητές από το αρχείο .env στη μνήμη
load_dotenv()

app = Flask(__name__)


def get_db_connection():
    # Κοιτάζει αν τρέχουμε στο Render (ψάχνει το DB_HOST του Render)
    # Αν δεν το βρει, σημαίνει ότι είμαστε στο PC σου
    if os.getenv("RENDER") or os.getenv("DB_HOST"):
        # Κώδικας για το Live Render
        connection = mysql.connector.connect(
            host=os.getenv("DB_HOST"),
            user=os.getenv("DB_USER"),
            password=os.getenv("DB_PASSWORD"),
            database=os.getenv("DB_NAME"),
            port=int(os.getenv("DB_PORT", 3306))
        )
    return connection

# --- ΣΥΝΑΡΤΗΣΗ ΓΙΑ ΑΥΤΟΜΑΤΟ ΣΤΗΣΙΜΟ ΤΩΝ ΠΙΝΑΚΩΝ ---
def init_db():
    try:
        conn = get_db_connection()
        cursor = conn.cursor()
        
        # Αν υπάρχει το αρχείο schema.sql, τρέξε το για να φτιαχτεί ο πίνακας applications
        if os.path.exists('schema.sql'):
            with open('schema.sql', 'r', encoding='utf-8') as f:
                # Χωρίζουμε τις εντολές SQL με βάση το ερωτηματικό ;
                sql_commands = f.read().split(';')
                
            for command in sql_commands:
                if command.strip():
                    cursor.execute(command)
            conn.commit()
            print("🎉 Η βάση δεδομένων ενημερώθηκε επιτυχώς!")
        
        cursor.close()
        conn.close()
    except Exception as e:
        print(f"❌ Σφάλμα κατά την προετοιμασία της βάσης: {e}")

# 1. READ: Εμφάνιση όλων των αιτήσεων
@app.route('/')
def index():
    conn = get_db_connection()
    cursor = conn.cursor(dictionary=True)
    cursor.execute("SELECT * FROM applications ORDER BY app_date DESC")
    jobs = cursor.fetchall()
    cursor.close()
    conn.close()
    return render_template('index.html', jobs=jobs)

# 2. CREATE: Προσθήκη νέας αίτησης
@app.route('/add', methods=['POST'])
def add_job():
    company = request.form['company_name']
    title = request.form['job_title']
    date = request.form['app_date']
    
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute(
        "INSERT INTO applications (company_name, job_title, app_date) VALUES (%s, %s, %s)",
        (company, title, date)
    )
    conn.commit()
    cursor.close()
    conn.close()
    return redirect(url_for('index'))

# 3. DELETE: Διαγραφή αίτησης με βάση το ID
@app.route('/delete/<int:id>')
def delete_job(id):
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute("DELETE FROM applications WHERE id = %s", (id,))
    conn.commit()
    cursor.close()
    conn.close()
    return redirect(url_for('index'))

# 4. UPDATE: Αλλαγή του Status
@app.route('/update/<int:id>', methods=['POST'])
def update_status(id):
    new_status = request.form['status']
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute("UPDATE applications SET status = %s WHERE id = %s", (new_status, id))
    conn.commit()
    cursor.close()
    conn.close()
    return redirect(url_for('index'))

if __name__ == '__main__':
    # Καλούμε το init_db() ΕΔΩ, ώστε να φτιαχτεί ο πίνακας ΠΡΙΝ ξεκινήσει ο server
    print("⏳ Γίνεται έλεγχος και προετοιμασία της βάσης δεδομένων στο Aiven...")
    init_db()
    
    # Μετά ξεκινάει το Flask app
    app.run(debug=True)