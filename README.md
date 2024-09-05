using System;
using System.Collections.Generic;
using System.Linq;

public class HelpDeskManagementSystem
{
    private static List<Ticket> tickets = new List<Ticket>();
    private static int nextTicketId = 1;

    public static void Main(string[] args)
    {
        Console.WriteLine("Welcome to the Help Desk Management System!");

        while (true)
        {
            Console.WriteLine("nChoose an option:");
            Console.WriteLine("1. Create Ticket");
            Console.WriteLine("2. View Tickets");
            Console.WriteLine("3. Update Ticket");
            Console.WriteLine("4. Close Ticket");
            Console.WriteLine("5. Exit");

            int choice = GetIntInput("Enter your choice: ");

            switch (choice)
            {
                case 1:
                    CreateTicket();
                    break;
                case 2:
                    ViewTickets();
                    break;
                case 3:
                    UpdateTicket();
                    break;
                case 4:
                    CloseTicket();
                    break;
                case 5:
                    Console.WriteLine("Exiting the system. Goodbye!");
                    return;
                default:
                    Console.WriteLine("Invalid choice. Please try again.");
                    break;
            }
        }
    }

    private static void CreateTicket()
    {
        Console.WriteLine("nCreating Ticket");
        Console.Write("Enter the subject: ");
        string subject = Console.ReadLine();
        Console.Write("Enter the description: ");
        string description = Console.ReadLine();
        Console.Write("Enter the priority (Low, Medium, High): ");
        string priority = Console.ReadLine().ToUpper();

        Ticket newTicket = new Ticket(nextTicketId++, subject, description, priority);
        tickets.Add(newTicket);
        Console.WriteLine("Ticket created successfully!");
        Console.WriteLine($"Ticket ID: {newTicket.Id}");
    }

    private static void ViewTickets()
    {
        Console.WriteLine("nTickets:");
        if (tickets.Count == 0)
        {
            Console.WriteLine("No tickets available.");
        }
        else
        {
            foreach (Ticket ticket in tickets)
            {
                Console.WriteLine($"Ticket ID: {ticket.Id}");
                Console.WriteLine($"Subject: {ticket.Subject}");

                Console.WriteLine($"Status: {ticket.Status}");
                Console.WriteLine($"Priority: {ticket.Priority}");
                Console.WriteLine("---------------------");
            }
        }
    }

    private static void UpdateTicket()
    {
        Console.WriteLine("nUpdating Ticket");
        Console.Write("Enter the ticket ID: ");
        int ticketId = GetIntInput("");

        Ticket ticketToUpdate = tickets.FirstOrDefault(t => t.Id == ticketId);

        if (ticketToUpdate != null)
        {
            Console.Write("Enter the new status (Open, In Progress, Resolved): ");
            string newStatus = Console.ReadLine().ToUpper();
            ticketToUpdate.Status = newStatus;
            Console.WriteLine("Ticket updated successfully.");
        }
        else
        {
            Console.WriteLine("Ticket not found.");
        }
    }

    private static void CloseTicket()
    {
        Console.WriteLine("nClosing Ticket");
        Console.Write("Enter the ticket ID: ");
        int ticketId = GetIntInput("");

        Ticket ticketToClose = tickets.FirstOrDefault(t => t.Id == ticketId);

        if (ticketToClose != null)
        {
            ticketToClose.Status = "Closed";
            Console.WriteLine("Ticket closed successfully.");
        }
        else
        {
            Console.WriteLine("Ticket not found.");
        }
    }

    // Helper method to get integer input from the user
    private static int GetIntInput(string prompt)
    {
        while (true)
        {
            Console.Write(prompt);
            if (int.TryParse(Console.ReadLine(), out int input))
            {
                return input;
            }
            Console.WriteLine("Invalid input. Please enter a number.");
        }
    }
}

// Ticket class
public class Ticket
{
    public int Id { get; private set; }
    public string Subject { get; set; }
    public string Description { get; set; }
    public string Priority { get; set; }
    public string Status { get; set; } = "Open"; // Default status is Open

    public Ticket(int id, string subject, string description, string priority)
    {
        Id = id;
        Subject = subject;
        Description = description;
        Priority = priority;
    }
}
