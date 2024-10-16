# Alumni-Network-System-java
import java.util.*;

class Alumni {
    String name;
    int yearOfAdmission;
    int yearOfGraduation;
    String contactNo;
    String emailAddress;
    String occupation;
    String address;

    Alumni(String name, int yearOfAdmission, int yearOfGraduation, String contactNo,
            String emailAddress, String occupation, String address) {
        this.name = name;
        this.yearOfAdmission = yearOfAdmission;
        this.yearOfGraduation = yearOfGraduation;
        this.contactNo = contactNo;
        this.emailAddress = emailAddress;
        this.occupation = occupation;
        this.address = address;
    }
}

public class AlumniInformationSystem {
    private static final Map<String, Alumni> alumniMap = new HashMap<>();
    private static final String[] firstNames = {"John", "Jane", "David", "Emily", "Michael", "Sarah", "Chris", "Anna"};
    private static final String[] lastNames = {"Doe", "Smith", "Brown", "Davis", "Johnson", "Taylor", "Lee", "Martin"};
    private static final String[] occupations = {"Software Engineer", "Data Scientist", "Product Manager", "Marketing Director", "Sales Executive"};
    private static final String[] streets = {"Elm St", "Oak St", "Pine St", "Maple St", "Birch St"};
    
    public static void main(String[] args) {
        System.out.println("Welcome to the Alumni Information System");
        
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("\nOptions:");
            System.out.println("1. Add Alumni Data");
            System.out.println("2. Check Alumni Details");
            System.out.println("3. Update Alumni Data");
            System.out.println("4. Delete Alumni Data");
            System.out.println("5. List All Alumni");
            System.out.println("6. Exit");

            System.out.print("Enter your choice (1-6): ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    addAlumniData(scanner);
                    break;
                case 2:
                    checkAlumniDetails(scanner);
                    break;
                case 3:
                    updateAlumniData(scanner);
                    break;
                case 4:
                    deleteAlumniData(scanner);
                    break;
                case 5:
                    listAllAlumni();
                    break;
                case 6:
                    System.out.println("Thank you for using the Alumni Information System. Goodbye!");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid choice! Please try again.");
            }
        }
    }

    private static void addAlumniData(Scanner scanner) {
        System.out.print("Number of students whose data you want to add: ");
        int n = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        for (int i = 0; i < n; i++) {
            String adm;
            while (true) {
                System.out.print("Enter admission number (5 digits): ");
                adm = scanner.nextLine();
                if (adm.matches("\\d{5}") && !alumniMap.containsKey(adm)) {
                    break;
                }
                System.out.println("Admission number must be 5 digits and unique.");
            }

            System.out.print("Enter name of the alumni: ");
            String name = scanner.nextLine();
            System.out.print("Enter Year of Admission: ");
            int yoa = scanner.nextInt();
            System.out.print("Enter Year of Graduation: ");
            int yog = yoa + 4; // Assuming graduation is 4 years after admission
            scanner.nextLine(); // Consume newline

            String contact;
            while (true) {
                System.out.print("Enter Contact number: ");
                contact = scanner.nextLine();
                if (isValidContact(contact)) {
                    break;
                }
                System.out.println("Entered contact number is invalid");
            }

            String email;
            while (true) {
                System.out.print("Enter registered e-mail address: ");
                email = scanner.nextLine();
                if (isValidEmail(email)) {
                    break;
                }
                System.out.println("This email address is not valid");
            }

            System.out.print("Enter current occupation: ");
            String occupation = scanner.nextLine();
            System.out.print("Enter current address: ");
            String address = scanner.nextLine();

            alumniMap.put(adm, new Alumni(name, yoa, yog, contact, email, occupation, address));
            System.out.println("Alumni data added successfully!");
        }
    }

    private static void checkAlumniDetails(Scanner scanner) {
        System.out.print("Number of students whose data you want to check: ");
        int x = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        for (int i = 0; i < x; i++) {
            System.out.print("Enter Admission number: ");
            String adm = scanner.nextLine();
            if (alumniMap.containsKey(adm)) {
                Alumni alumni = alumniMap.get(adm);
                System.out.println("\nName: " + alumni.name);
                System.out.println("Year of Admission: " + alumni.yearOfAdmission);
                System.out.println("Year of Graduation: " + alumni.yearOfGraduation);
                System.out.println("Contact No.: " + alumni.contactNo);
                System.out.println("E-mail Address: " + alumni.emailAddress);
                System.out.println("Occupation: " + alumni.occupation);
                System.out.println("Address: " + alumni.address + "\n");
            } else {
                System.out.println("Entered admission number is not registered in 'Alumni Information System'");
            }
        }
    }

    private static void updateAlumniData(Scanner scanner) {
        System.out.print("Enter Admission number to update: ");
        String adm = scanner.nextLine();
        if (alumniMap.containsKey(adm)) {
            Alumni alumni = alumniMap.get(adm);
            System.out.println("Updating data for " + alumni.name + ":");

            System.out.print("Enter new contact number (or press Enter to skip): ");
            String newContact = scanner.nextLine();
            if (!newContact.isEmpty() && isValidContact(newContact)) {
                alumni.contactNo = newContact;
            }

            System.out.print("Enter new email address (or press Enter to skip): ");
            String newEmail = scanner.nextLine();
            if (!newEmail.isEmpty() && isValidEmail(newEmail)) {
                alumni.emailAddress = newEmail;
            }

            System.out.print("Enter new occupation (or press Enter to skip): ");
            String newOccupation = scanner.nextLine();
            if (!newOccupation.isEmpty()) {
                alumni.occupation = newOccupation;
            }

            System.out.print("Enter new address (or press Enter to skip): ");
            String newAddress = scanner.nextLine();
            if (!newAddress.isEmpty()) {
                alumni.address = newAddress;
            }

            System.out.println("Alumni data updated successfully!");
        } else {
            System.out.println("This admission number is not found in the system.");
        }
    }

    private static void deleteAlumniData(Scanner scanner) {
        System.out.print("Enter Admission number to delete: ");
        String adm = scanner.nextLine();
        if (alumniMap.containsKey(adm)) {
            alumniMap.remove(adm);
            System.out.println("Alumni data for Admission number " + adm + " has been deleted.");
        } else {
            System.out.println("This admission number is not found in the system.");
        }
    }

    private static void listAllAlumni() {
        System.out.println("\nTotal number of alumni: " + alumniMap.size());
        System.out.println("\nList of all alumni:");
        for (Map.Entry<String, Alumni> entry : alumniMap.entrySet()) {
            String adm = entry.getKey();
            Alumni alumni = entry.getValue();
            System.out.println("\nAdmission Number: " + adm);
            System.out.println("Name: " + alumni.name);
            System.out.println("Year of Admission: " + alumni.yearOfAdmission);
            System.out.println("Year of Graduation: " + alumni.yearOfGraduation);
            System.out.println("Contact No.: " + alumni.contactNo);
            System.out.println("E-mail Address: " + alumni.emailAddress);
            System.out.println("Occupation: " + alumni.occupation);
            System.out.println("Address: " + alumni.address);
        }
    }

    private static boolean isValidContact(String contact) {
        return contact.matches("\\d{10}");
    }

    private static boolean isValidEmail(String email) {
        return email.contains("@") && email.contains(".");
    }
}
