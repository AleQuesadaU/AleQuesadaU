# Importar módulos necesarios
import random
import string
from datetime import datetime, timedelta

# Definir variables globales
HOTEL_NAME = "Hotel California"
ROOMS_AVAILABLE = 20
RESERVATIONS = []

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

    def available_rooms(self):
        return self.rooms - len(RESERVATIONS)

    def make_reservation(self, guest_name, check_in_date, check_out_date):
        if self.available_rooms() == 0:
            print("No hay habitaciones disponibles.")
            return

        # Generar un número de habitación aleatorio
        room_number = random.randint(1, self.rooms)

        # Crear una nueva reserva
        reservation = Reservation(guest_name, check_in_date, check_out_date, room_number)
        RESERVATIONS.append(reservation)

        print(f"Reservación realizada: {reservation}")

    def cancel_reservation(self, guest_name):
        for reservation in RESERVATIONS:
            if reservation.guest_name == guest_name:
                RESERVATIONS.remove(reservation)
                print(f"Reservación cancelada: {reservation}")
                return
        print("No se encontró ninguna reservación con ese nombre.")

# Función para imprimir una lista de reservaciones
def print_reservations(reservations):
    print("Reservaciones:")
    for reservation in reservations:
        print(f"- {reservation}")

# Función para generar una cadena aleatoria
def random_string(length):
    return ''.join(random.choice(string.ascii_letters) for i in range(length))

# Definir variables locales
hotel = Hotel(HOTEL_NAME, ROOMS_AVAILABLE)
today = datetime.today()

# Interacciones con el usuario
print(f"Bienvenido al {hotel.name}!")
print(f"Hay {hotel.available_rooms()} habitaciones disponibles.")

while True:
    print("¿Qué acción desea realizar?")
    print("1. Realizar una reservación")
    print("2. Cancelar una reservación")
    print("3. Ver todas las reservaciones")
    print("4. Salir")
    choice = input("> ")

    if choice == "1":
        print("Ingrese su nombre:")
        guest_name = input("> ")
        print("Ingrese la fecha de check-in (yyyy-mm-dd):")
        check_in_date = input("> ")
        print("Ingrese la fecha de check-out (yyyy-mm-dd):")
        check_out_date = input("> ")
        hotel.make_reservation(guest_name, check_in_date, check_out_date)
    elif choice == "2":
        print("Ingrese su nombre:")
        guest_name = input("> ")
        hotel.cancel_reservation(guest_name)
    elif choice == "3":
        print_reservations(RESERVATIONS)
    elif choice == "4":
        break
