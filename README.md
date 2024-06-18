#include <iostream>   // For standard I/O operations
#include <fstream>    // For file handling
#include <map>        // To use the std::map data structure
#include <string>     // To handle strings
#include <limits>     // For std::numeric_limits

// ItemTracker class definition
class ItemTracker
{
    private:
    // Map to store item frequencies
    std::map<std::string, int> itemFrequency;

    // Private method to read input file and populate the map
    void readFile()
    {
        std::ifstream infile("CS210_Project_Three_Input_File.txt");
        std::string item;
        if (!infile)
        {
            std::cerr << "Error opening file!" << std::endl;
            return;
        }
        // Read each item from the file and update its frequency in the map
        while (infile >> item)
        {
            itemFrequency[item]++;
        }
        infile.close();
    }

    // Private method to write the map data to a backup file
    void writeBackupFile()
    {
        std::ofstream outfile("frequency.dat");
        if (!outfile)
        {
            std::cerr << "Error opening backup file!" << std::endl;
            return;
        }
        // Write each item and its frequency to the backup file
        for (const auto&pair : itemFrequency) {
            outfile << pair.first << " " << pair.second << std::endl;
        }
        outfile.close();
    }

    public:
    // Constructor initializes the map by reading the input file and creating the backup file
    ItemTracker()
    {
        readFile();
        writeBackupFile();
    }

    // Method to get the frequency of a specific item
    int getItemFrequency(const std::string& item) {
        // If item exists in the map, return its frequency; otherwise, return 0
        if (itemFrequency.find(item) != itemFrequency.end()) {
            return itemFrequency[item];
        }
        return 0;
    }

    // Method to print the frequency of all items
    void printAllFrequencies()
{
    // Iterate through the map and print each item and its frequency
    for (const auto&pair : itemFrequency) {
        std::cout << pair.first << " " << pair.second << std::endl;
    }
}

// Method to print a histogram of item frequencies
void printHistogram()
{
    // Iterate through the map and print each item followed by a number of asterisks equal to its frequency
    for (const auto&pair : itemFrequency) {
        std::cout << pair.first << " ";
        for (int i = 0; i < pair.second; ++i)
        {
            std::cout << "*";
        }
        std::cout << std::endl;
    }
}
};

// Main function to drive the program
int main()
{
    ItemTracker tracker; // Create an instance of ItemTracker
    int choice;
    std::string item;

    do
    {
        // Display menu options
        std::cout << "Menu Options:\n";
        std::cout << "1. Find frequency of a specific item\n";
        std::cout << "2. Print frequency of all items\n";
        std::cout << "3. Print histogram of all items\n";
        std::cout << "4. Exit\n";
        std::cout << "Enter your choice: ";
        std::cin >> choice;

        // Input validation for menu choice
        while (std::cin.fail() || choice < 1 || choice > 4)
        {
            std::cin.clear(); // Clear the error flag
            std::cin.ignore(std::numeric_limits < std::streamsize >::max(), '\n'); // Ignore invalid input
            std::cout << "Invalid input. Please enter a number between 1 and 4: ";
            std::cin >> choice;
        }

        switch (choice)
        {
            case 1:
                // Prompt user to enter an item and display its frequency
                std::cout << "Enter item name: ";
                std::cin >> item;
                std::cout << "Frequency of " << item << ": " << tracker.getItemFrequency(item) << std::endl;
                break;
            case 2:
                // Print the frequency of all items
                tracker.printAllFrequencies();
                break;
            case 3:
                // Print the histogram of all items
                tracker.printHistogram();
                break;
            case 4:
                // Exit the program
                std::cout << "Exiting program." << std::endl;
                break;
        }
    } while (choice != 4);

    return 0;
}
