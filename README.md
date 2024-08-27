#include <iostream>
#include <filesystem>
#include <string>
#include <vector>

namespace fs = std::filesystem;

// Function declarations
void displayMainMenu();
void listFilesMenu();
void createDirectory();
void changeDirectory();
void listAllFiles();
void listFilesByExtension();
void listFilesByPattern();

int main() {
    while (true) {
        displayMainMenu();
        int choice;
        std::cin >> choice;

        switch (choice) {
            case 1:
                listFilesMenu();
                break;
            case 2:
                createDirectory();
                break;
            case 3:
                changeDirectory();
                break;
            case 4:
                std::cout << "Exiting the program." << std::endl;
                return 0;
            default:
                std::cout << "Invalid choice. Please try again." << std::endl;
        }
    }
}

void displayMainMenu() {
    std::cout << "Main Menu:" << std::endl;
    std::cout << "1. List files in the current directory." << std::endl;
    std::cout << "2. Create a new directory." << std::endl;
    std::cout << "3. Change the working directory." << std::endl;
    std::cout << "4. Exit the program." << std::endl;
    std::cout << "Enter your choice: ";
}

void listFilesMenu() {
    std::cout << "List Files Menu:" << std::endl;
    std::cout << "1. List all files in the current directory." << std::endl;
    std::cout << "2. List files based on a specific extension (e.g., .txt)." << std::endl;
    std::cout << "3. List files based on a pattern (e.g., moha*.*)." << std::endl;
    std::cout << "Enter your choice: ";

    int choice;
    std::cin >> choice;

    switch (choice) {
        case 1:
            listAllFiles();
            break;
        case 2:
            listFilesByExtension();
            break;
        case 3:
            listFilesByPattern();
            break;
        default:
            std::cout << "Invalid choice. Returning to the main menu." << std::endl;
    }
}

void listAllFiles() {
    std::cout << "Files in the current directory:" << std::endl;
    for (const auto& entry : fs::directory_iterator(fs::current_path())) {
        if (fs::is_regular_file(entry.status())) {
            std::cout << entry.path().filename().string() << std::endl;
        }
    }
}

void listFilesByExtension() {
    std::cout << "Enter the file extension (e.g., .txt): ";
    std::string extension;
    std::cin >> extension;

    std::cout << "Files with extension " << extension << ":" << std::endl;
    for (const auto& entry : fs::directory_iterator(fs::current_path())) {
        if (fs::is_regular_file(entry.status()) && entry.path().extension() == extension) {
            std::cout << entry.path().filename().string() << std::endl;
        }
    }
}

void listFilesByPattern() {
    std::cout << "Enter the pattern (e.g., moha*.*): ";
    std::string pattern;
    std::cin >> pattern;

    std::cout << "Files matching pattern " << pattern << ":" << std::endl;
    for (const auto& entry : fs::directory_iterator(fs::current_path())) {
        if (fs::is_regular_file(entry.status()) && entry.path().filename().string().find(pattern) != std::string::npos) {
            std::cout << entry.path().filename().string() << std::endl;
        }
    }
}

void createDirectory() {
    std::cout << "Enter the name of the directory to create: ";
    std::string dirName;
    std::cin >> dirName;

    if (fs::create_directory(dirName)) {
        std::cout << "Directory created successfully." << std::endl;
    } else {
        std::cout << "Failed to create directory. It may already exist." << std::endl;
    }
}

void changeDirectory() {
    std::cout << "Change Directory Menu:" << std::endl;
    std::cout << "1. Move one step back (to the parent directory)." << std::endl;
    std::cout << "2. Move to the root directory." << std::endl;
    std::cout << "3. Move to a specific directory provided by the user." << std::endl;
    std::cout << "Enter your choice: ";

    int choice;
    std::cin >> choice;

    switch (choice) {
        case 1:
            fs::current_path(fs::current_path().parent_path());
            std::cout << "Moved to parent directory: " << fs::current_path() << std::endl;
            break;
        case 2:
            fs::current_path(fs::path("/"));
            std::cout << "Moved to root directory: " << fs::current_path() << std::endl;
            break;
        case 3: {
            std::cout << "Enter the directory path: ";
            std::string path;
            std::cin >> path;

            if (fs::exists(path) && fs::is_directory(path)) {
                fs::current_path(path);
                std::cout << "Moved to directory: " << fs::current_path() << std::endl;
            } else {
                std::cout << "Directory does not exist." << std::endl;
            }
            break;
        }
        default:
            std::cout << "Invalid choice. Returning to the main menu." << std::endl;
    }
}
