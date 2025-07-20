# Final-Summer-Project-
# Code: 

import random
import csv
import os
from datetime import datetime

# --- Class Definition for the Game ---
class NumberGuessGame:
    def __init__(self, max_number=100, max_attempts=10):
        # Initialize game settings
        self.max_number = max_number            # Maximum number that can be guessed
        self.max_attempts = max_attempts        # Maximum number of guesses allowed
        self.secret_number = random.randint(1, max_number)  # Random number to guess
        self.attempts = 0                        # Counter for number of attempts
        self.history_file = 'game_history.csv'   # File to store game results

    def play(self):
        # Start the game and prompt the user
        print(f"Welcome to the Number Guessing Game!")
        print(f"I'm thinking of a number between 1 and {self.max_number}.")
        print(f"You have {self.max_attempts} attempts to guess it.\n")

        # Loop until the user runs out of attempts
        while self.attempts < self.max_attempts:
            try:
                # Get user input and convert to integer
                guess = int(input("Enter your guess: "))
            except ValueError:
                # If input is not a number, show error and continue
                print("Please enter a valid number.")
                continue

            self.attempts += 1  # Increase attempt count

            # Check guess and provide feedback
            if guess == self.secret_number:
                print(f"Congratulations! You guessed it in {self.attempts} attempts.")
                self.save_result("Win") 
                break
            elif guess < self.secret_number:
                print("Too low! Try again.")
            else:
                print("Too high! Try again.")

        else:
            # If the loop ends without a correct guess
            print(f" You've used all {self.max_attempts} attempts. The number was {self.secret_number}.")
            self.save_result("Lose")  # Save the result as a loss

    def save_result(self, result):
        # Save the game result in a CSV file
        file_exists = os.path.exists(self.history_file)  # Check if file already exists

        
        with open(self.history_file, mode='a', newline='') as file:
            writer = csv.writer(file)

            # If the file is new, write the header row
            if not file_exists:
                writer.writerow(["Date", "Result", "Attempts", "SecretNumber"])

            # Write the current game's data
            writer.writerow([
                datetime.now().strftime("%Y-%m-%d %H:%M:%S"),  # Current date/time
                result,         # "Win" or "Lose"
                self.attempts,  # Number of attempts used
                self.secret_number  # The correct number
            ])
        print("Game result saved to game_history.csv\n")

# --- Main Program Execution ---
if __name__ == "__main__":
    game = NumberGuessGame() 
    game.play() 
# Output: 
Welcome to the Number Guessing Game!
I'm thinking of a number between 1 and 100.
You have 10 attempts to guess it.

Enter your guess: 99
Too high! Try again.
Enter your guess: 85
Too high! Try again.
Enter your guess: 68
Too high! Try again.
Enter your guess: 58
Too high! Try again.
Enter your guess: 2
Too low! Try again.
Enter your guess: 13
Too low! Try again.
Enter your guess: 28
Too low! Try again.
Enter your guess: 38
Too low! Try again.
Enter your guess: 48
Too high! Try again.
Enter your guess: 45
Too high! Try again.
 You've used all 10 attempts. The number was 42.
Game result saved to game_history.csv
