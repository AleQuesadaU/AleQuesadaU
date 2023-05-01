# Importar librerías necesarias
import random
from datetime import datetime, timedelta

# Clase de Reservación
class Reservation:
    def __init__(self, guest_name, check_in_date, check_out_date, room_number):
        self.guest_name = guest_name
        self.check_in_date = check_in_date
        self.check_out_date = check_out_date
        self.room_number = room_number

    def __str__(self):
        return f"{self.guest_name} - Room {self.room_number}, {self.check_in_date} to {self.check_out_date}"

# Clase de Hotel
class Hotel:
    def __init__(self, name, rooms):
        self.name = name
        self.rooms = rooms
        self.reservations = []

    def available_rooms(self):
        return self.rooms - len(self.reservations)

    def make_reservation(self, guest_name, check_in_date, check_out_date):
        if self.available_rooms() == 0:
            raise Exception("No hay habitaciones disponibles.")
        
        # Generar un número de habitación aleatorio
        room_number = random.randint(1, self.rooms)

        # Crear una nueva reserva
        reservation = Reservation(guest_name, check_in_date, check_out_date, room_number)
        self.reservations.append(reservation)

        print(f"Reservación realizada: {reservation}")

    def cancel_reservation(self, guest_name):
        for reservation in self.reservations:
            if reservation.guest_name == guest_name:
                self.reservations.remove(reservation)
                print(f"Reservación cancelada: {reservation}")
                return
        print("No se encontró ninguna reservación con ese nombre.")

    def get_reservation_by_date(self, check_in_date):
        reservations = []
        for reservation in self.reservations:
            if reservation.check_in_date == check_in_date:
                reservations.append(reservation)
        return reservations

    def __iter__(self):
        return iter(self.reservations)

# Función para imprimir una lista de reservaciones
def print_reservations(reservations):
    if not reservations:
        print("No hay reservaciones para la fecha dada.")
        return
    print("Reservaciones:")
    for reservation in reservations:
        print(f"- {reservation}")

# Función para convertir una cadena a fecha
def str_to_date(date_string):
    try:
        date = datetime.strptime(date_string, '%Y-%m-%d')
        return date
    except ValueError:
        raise ValueError("Formato de fecha inválido. Debe ser en formato yyyy-mm-dd.")

# Interacciones con el usuario
print("Bienvenido al Hotel California!")

while True:
    print("\n¿Qué acción desea realizar?")
    print("1. Realizar una reservación")
    print("2. Cancelar una reservación")
    print("3. Ver todas las reservaciones")
    print("4. Ver reservaciones por fecha")
    print("5. Salir")
    choice = input("> ")

    if choice == "1":
        try:
            print("Ingrese su nombre:")
            guest_name = input("> ")
            print("Ingrese la fecha de check-in (yyyy-mm-dd):")
            check_in_date_string = input("> ")
            check_in_date = str_to_date(check_in_date_string)
            print("Ingrese la fecha de check-out (yyyy-mm-dd:")
