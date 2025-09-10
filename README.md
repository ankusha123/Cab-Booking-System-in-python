# Cab-Booking-System-in-python
simple Cab Booking System in Python (console-based) to help you get started. It includes core functionality like:  Viewing available cabs  Booking a cab  Viewing bookings  Canceling a booking  This is basic, modular, and uses in-memory storage (no database or file I/O), so you can build on it later.

class Cab:
    def __init__(self, cab_id, driver_name, location, is_available=True):
        self.cab_id = cab_id
        self.driver_name = driver_name
        self.location = location
        self.is_available = is_available

    def __str__(self):
        status = "Available" if self.is_available else "Booked"
        return f"Cab ID: {self.cab_id} | Driver: {self.driver_name} | Location: {self.location} | Status: {status}"


class CabBookingSystem:
    def __init__(self):
        self.cabs = []
        self.bookings = []

    def add_cab(self, cab):
        self.cabs.append(cab)

    def show_available_cabs(self):
        available = [cab for cab in self.cabs if cab.is_available]
        if not available:
            print("No cabs available at the moment.")
        else:
            print("\n--- Available Cabs ---")
            for cab in available:
                print(cab)

    def book_cab(self, user_name, pickup_location):
        for cab in self.cabs:
            if cab.is_available and cab.location.lower() == pickup_location.lower():
                cab.is_available = False
                self.bookings.append({
                    "user": user_name,
                    "cab_id": cab.cab_id,
                    "driver": cab.driver_name,
                    "pickup": pickup_location
                })
                print(f"\n✅ Booking Confirmed for {user_name} with Cab ID {cab.cab_id} (Driver: {cab.driver_name})")
                return
        print("❌ No available cabs at your location.")

    def view_bookings(self):
        if not self.bookings:
            print("No bookings yet.")
        else:
            print("\n--- All Bookings ---")
            for b in self.bookings:
                print(f"User: {b['user']} | Cab ID: {b['cab_id']} | Driver: {b['driver']} | Pickup: {b['pickup']}")

    def cancel_booking(self, cab_id):
        for booking in self.bookings:
            if booking['cab_id'] == cab_id:
                self.bookings.remove(booking)
                for cab in self.cabs:
                    if cab.cab_id == cab_id:
                        cab.is_available = True
                        print(f"🚫 Booking for Cab ID {cab_id} has been cancelled.")
                        return
        print("❌ Booking not found.")


# --- Main Program ---
def main():
    system = CabBookingSystem()

    # Sample cabs
    system.add_cab(Cab(101, "Alice", "Downtown"))
    system.add_cab(Cab(102, "Bob", "Airport"))
    system.add_cab(Cab(103, "Charlie", "Downtown"))
    system.add_cab(Cab(104, "David", "Uptown"))

    while True:
        print("\n===== Cab Booking System =====")
        print("1. Show Available Cabs")
        print("2. Book a Cab")
        print("3. View All Bookings")
        print("4. Cancel a Booking")
        print("5. Exit")

        choice = input("Enter your choice (1-5): ")

        if choice == '1':
            system.show_available_cabs()
        elif choice == '2':
            user = input("Enter your name: ")
            location = input("Enter your pickup location: ")
            system.book_cab(user, location)
        elif choice == '3':
            system.view_bookings()
        elif choice == '4':
            try:
                cab_id = int(input("Enter Cab ID to cancel: "))
                system.cancel_booking(cab_id)
            except ValueError:
                print("Please enter a valid Cab ID.")
        elif choice == '5':
            print("Thank you for using the Cab Booking System. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()

