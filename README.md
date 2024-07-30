

# Election Management System

## Overview

The **Election Management System** is a C++ project designed to streamline the process of conducting elections. Utilizing Object-Oriented Programming (OOP) principles, this system supports:

- Candidate registration
- Anonymous voting
- Displaying election results, including handling ties

This project is ideal for educational purposes and small-scale election scenarios.

## Features

### 1. Authentication

**Purpose:** Prevent unauthorized access to the system.

**Implementation:**

- **Function:** `authenticate()`
- **Description:** Prompts the user for a username and password. If credentials match predefined values, access is granted; otherwise, the program exits.

**Code:**
   
    bool authenticate() {
    const string USERNAME = "beast";
    const string PASSWORD = "1234";
    string enteredUsername, enteredPassword;

    cout << "\nAUTHENTICATION\n";
    cout << "Enter username: ";
    cin >> enteredUsername;
    cout << "Enter password: ";
    cin >> enteredPassword;

    return (enteredUsername == USERNAME && enteredPassword == PASSWORD);
}

 
### 2. Candidate Registration
Purpose: Add candidates to the election with unique names.

Implementation:

Method addCandidate() checks if the candidate name already exists.
Prompts for a new name if a duplicate is found.
Limits the number of attempts to add a unique candidate name.
Code Reference:

**Code:**

    void addCandidate(string candidateName) {
    const int maxAttempts = 3;
    int attempts = 0;
    while (true) {
        bool nameExists = false;
        for (const auto& candidate : candidates) {
            if (candidate.getName() == candidateName) {
                nameExists = true;
                cout << "Error: A candidate with the name '" << candidateName << "' already exists.\n";
                cout << "Please enter a new name: ";
                getline(cin, candidateName);
                attempts++;
                break;
            }
        }
        if (!nameExists) {
            candidates.emplace_back(candidateName);
            break;
        }
        if (attempts >= maxAttempts) {
            cout << "Too many attempts. Candidate not added.\n";
            break;
        }
    }
    }
### 3. Anonymous Voting
Purpose: Allow voters to cast their votes anonymously.

Implementation:

Method conductElection() prompts for the number of voters.
Displays the list of candidates.
Allows each voter to vote by selecting a candidate number.
Ensures only valid votes are counted by verifying voter input.
Code Reference:

**Code:**

    void conductElection() {
    int numVoters;
    cout << "\nEnter the number of voters: ";
    cin >> numVoters;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    displayCandidates();

    cout << "\nVote for the candidate (Choose Candidate number)\n\n";

    for (int i = 0; i < numVoters; ++i) {
        while (true) {
            cout << "Anonymous Voter " << i + 1 << ": ";
            int choice;
            if (!(cin >> choice)) {
                cout << "Invalid input. Enter an integer.\n";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                continue;
            }

    if (choice < 1 || static_cast<size_t>(choice) > candidates.size()) {
                cout << "Invalid choice. Enter a valid candidate number.\n";
            } else {
                candidates[choice - 1].vote();
                break;
            }
        }
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
    }
    }
### 4. Displaying Candidates
Purpose: Show the list of candidates with their respective numbers.

Implementation:

Method displayCandidates() outputs the list of candidates with their assigned numbers.
Code Reference:

**Code:**

    void displayCandidates() const {
    cout << "\nCandidates:\n";
    cout << setw(10) << left << "Number" << setw(20) << left << "Name" << "\n";
    cout << string(30, '-') << "\n";
    for (size_t i = 0; i < candidates.size(); ++i) {
        cout << setw(10) << left << i + 1 << setw(20) << left << candidates[i].getName() << "\n";
    }
     }
### 5. Election Results Display
Purpose: Show the results of the election, including handling ties.

Implementation:

Method displayResults() outputs the number of votes each candidate received.
Determines the winner or handles a tie by displaying all candidates with the highest votes.
Code Reference:

**Code:**

    void displayResults() const {
    cout << endl;
    cout << "Election Results:\n";
    cout << setw(20) << left << "Candidate Name" << setw(10) << left << "Votes" << "\n";
    cout << string(30, '-') << "\n";

    int maxVotes = 0;

    for (const auto& candidate : candidates) {
        if (candidate.getVotes() > maxVotes) {
            maxVotes = candidate.getVotes();
        }
    }
    vector<string> winners;
    for (const auto& candidate : candidates) {
        cout << setw(20) << left << candidate.getName() << setw(10) << left << candidate.getVotes() << "\n";
        if (candidate.getVotes() == maxVotes) {
            winners.push_back(candidate.getName());
        }
    }

    if (winners.size() == 1) {
        cout << endl;
        cout << "The winner is " << winners[0] << " with " << maxVotes << " votes.\n";
    } else {
        cout << endl;
        cout << "It's a tie between:\n";
        for (const auto& winner : winners) {
            cout << winner << "\n";
        }
        cout << "Each with " << maxVotes << " votes.\n";
    }
    }




## Project Structure

The project is structured as follows:

- **`src/`**: Contains the source code files.
  - **`main.cpp`**: Contains the main function and integrates all components of the election system.
  - **`Candidate.h`**: Header file for the `Candidate` class, which represents a candidate in the election.
  - **`Candidate.cpp`**: Implementation of the `Candidate` class methods.
  - **`Election.h`**: Header file for the `Election` class, managing candidate registration, voting, and results.
  - **`Election.cpp`**: Implementation of the `Election` class methods.

- **`build/`**: Directory where build artifacts (compiled binaries) are placed. This directory is usually not included in version control.

- **`README.md`**: Provides an overview of the project, its features, installation instructions, and usage details.

- **`LICENSE`**: Contains the licensing information for the project.

- **`.gitignore`**: Specifies which files and directories should be ignored by Git (e.g., build artifacts, temporary files).

### Conclusion
The Election Management System project demonstrates a practical application of Object-Oriented Programming (OOP) principles in C++. By implementing a robust system for managing and conducting elections, this project showcases key functionalities including authentication, candidate registration, anonymous voting, and results display.

The system ensures secure access through a simple authentication mechanism, maintains the integrity of candidate registration by preventing duplicate entries, and provides an intuitive voting process for users. The results are presented in a clear and concise manner, handling both single and tied outcomes effectively.

Overall, this project not only fulfills the core requirements of an election management system but also serves as a strong example of how OOP concepts can be leveraged to create functional and user-friendly applications. Future enhancements could include integrating a graphical user interface, expanding to support more complex voting systems, or incorporating advanced security features. This foundational work lays the groundwork for such improvements, offering a solid basis for further development and exploration.



