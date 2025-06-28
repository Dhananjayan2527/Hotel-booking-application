# Hotel-booking-application
import java.util.Scanner;

class Room {
    int roomNumber;
    boolean isAvailable;
    String bookedBy;

    Room(int number) {
        roomNumber = number;
        isAvailable = true;
        bookedBy = null;
    }

    void showStatus() {
        if (isAvailable) {
            System.out.println("Room " + roomNumber + ": Available");
        } else {
            System.out.println("Room " + roomNumber + ": Booked by " + bookedBy);
        }
    }

    void book(String userName) {
        if (isAvailable) {
            System.out.println("Room " + roomNumber + " is available. Booking now...");
            System.out.println("Room " + roomNumber + " booked successfully for " + userName + "!");
            isAvailable = false;
            bookedBy = userName;
        } else {
            System.out.println("Sorry! Room " + roomNumber + " is already booked by " + bookedBy + ".");
        }
    }
}

public class HotelBookingSystem {

    static Scanner sc = new Scanner(System.in);
    static Room[] rooms = new Room[5];
    static String adminUsername = "admin";
    static String adminPassword = "admin123";

    public static void main(String[] args) {
        initializeRooms();

        System.out.println("===== Welcome to Hotel Booking Application =====");

        while (true) {
            System.out.print("\nLogin as (user/admin/exit): ");
            String role = sc.nextLine().toLowerCase();

            switch (role) {
                case "user":
                    handleUser();
                    break;
                case "admin":
                    handleAdmin();
                    break;
                case "exit":
                    System.out.println("Exiting Hotel Booking System. Goodbye!");
                    sc.close();
                    return;
                default:
                    System.out.println("Invalid role. Please enter 'user', 'admin', or 'exit'.");
            }
        }
    }

    static void initializeRooms() {
        rooms[0] = new Room(101);
        rooms[1] = new Room(102);
        rooms[2] = new Room(103);
        rooms[3] = new Room(104);
        rooms[4] = new Room(105);
    }

    static void handleUser() {
        System.out.print("Enter your name: ");
        String name = sc.nextLine();

        System.out.println("\nAvailable Rooms:");
        boolean anyAvailable = false;
        for (Room room : rooms) {
            if (room.isAvailable) {
                room.showStatus();
                anyAvailable = true;
            }
        }

        if (!anyAvailable) {
            System.out.println("Sorry, all rooms are booked.");
            return;
        }

        System.out.print("\nEnter room number to book: ");
        int roomChoice = sc.nextInt();
        sc.nextLine(); // Clear buffer

        boolean found = false;
        for (Room room : rooms) {
            if (room.roomNumber == roomChoice) {
                room.book(name);
                found = true;
                break;
            }
        }

        if (!found) {
            System.out.println("Invalid room number.");
        }

        System.out.println("Thank you, " + name + ", for visiting!");
    }

    static void handleAdmin() {
        System.out.print("Enter admin username: ");
        String username = sc.nextLine();
        System.out.print("Enter admin password: ");
        String password = sc.nextLine();

        if (username.equals(adminUsername) && password.equals(adminPassword)) {
            System.out.println("\n--- Booked Room Details ---");
            boolean anyBooked = false;
            for (Room room : rooms) {
                if (!room.isAvailable) {
                    System.out.println("Room " + room.roomNumber + " booked by " + room.bookedBy);
                    anyBooked = true;
                }
            }
            if (!anyBooked) {
                System.out.println("No rooms booked yet.");
            }
            System.out.println("Thank you, Admin.");
        } else {
            System.out.println("Incorrect username or password. Access denied.");
        }
    }
}
