import mysql.connector
from mysql.connector import Error

class Nastamba:
    def __init__(self, broj, vrsta, ime=None):
        self.broj = broj
        self.vrsta = vrsta
        self.ime = ime

class Zivotinja:
    def __init__(self, id, vrsta, ime, broj, nastamba_id):
        self.id = id
        self.vrsta = vrsta
        self.ime = ime
        self.broj = broj
        self.nastamba_id = nastamba_id

class Radnik:
    def __init__(self, ime, prezime, kontakt):
        self.ime = ime
        self.prezime = prezime
        self.kontakt = kontakt

class Trosak:
    def __init__(self, zivotinja_id, trosak_materijala, trosak_radnika):
        self.zivotinja_id = zivotinja_id
        self.trosak_materijala = trosak_materijala
        self.trosak_radnika = trosak_radnika

class Posjeta:
    def __init__(self, ime_grupe, vodic_id):
        self.ime_grupe = ime_grupe
        self.vodic_id = vodic_id

class Nezgoda:
    def __init__(self, vrsta, utjecaj, komentari, trosak_sanacije):
        self.vrsta = vrsta
        self.utjecaj = utjecaj
        self.komentari = komentari
        self.trosak_sanacije = trosak_sanacije

def create_connection(host_name, user_name, user_password, db_name):
    try:
        connection = mysql.connector.connect(
            host=host_name,
            user=user_name,
            passwd=user_password,
            database=db_name
        )
        print("Povezivanje s MySQL DB uspješno.")
        return connection
    except Error as e:
        print(f"Pogreška: '{e}'")
        return None

def execute_query(connection, query, data):
    cursor = connection.cursor()
    try:
        cursor.execute(query, data)
        connection.commit()
        print("Upit uspješno izvršen.")
    except Error as e:
        print(f"Pogreška: '{e}'")

connection = create_connection("localhost", "root", "tvoja_lozinka", "zoo_db")

nastamba1 = Nastamba(1, "Mračna", "Pećinska nastamba")
zivotinja1 = Zivotinja("A001", "Panthera leo", "Leo", 1, 1)
radnik1 = Radnik("Ivan", "Horvat", "ivan.horvat@zoo.hr")
trosak1 = Trosak("A001", 200, 50)
posjeta1 = Posjeta("Školski izlet", 1)
nezgoda1 = Nezgoda("Manja ozljeda", "Niska", "Životinja ozlijeđena tijekom hranjenja", 100)

nastamba_query = "INSERT INTO nastambe (broj, vrsta, ime) VALUES (%s, %s, %s)"
execute_query(connection, nastamba_query, (nastamba1.broj, nastamba1.vrsta, nastamba1.ime))

zivotinja_query = "INSERT INTO zivotinje (id, vrsta, ime, broj, nastamba_id) VALUES (%s, %s, %s, %s, %s)"
execute_query(connection, zivotinja_query, (zivotinja1.id, zivotinja1.vrsta, zivotinja1.ime, zivotinja1.broj, zivotinja1.nastamba_id))

radnik_query = "INSERT INTO radnici (ime, prezime, kontakt) VALUES (%s, %s, %s)"
execute_query(connection, radnik_query, (radnik1.ime, radnik1.prezime, radnik1.kontakt))

trosak_query = "INSERT INTO troskovi (zivotinja_id, trosak_materijala, trosak_radnika, ukupni_trosak) VALUES (%s, %s, %s, %s)"
execute_query(connection, trosak_query, (trosak1.zivotinja_id, trosak1.trosak_materijala, trosak1.trosak_radnika, trosak1.trosak_materijala + trosak1.trosak_radnika))

posjeta_query = "INSERT INTO posjete (ime_grupe, vodic_id) VALUES (%s, %s)"
execute_query(connection, posjeta_query, (posjeta1.ime_grupe, posjeta1.vodic_id))

nezgoda_query = "INSERT INTO nezgode (vrsta, utjecaj, komentari, trosak_sanacije) VALUES (%s, %s, %s, %s)"
execute_query(connection, nezgoda_query, (nezgoda1.vrsta, nezgoda1.utjecaj, nezgoda1.komentari, nezgoda1.trosak_sanacije))
