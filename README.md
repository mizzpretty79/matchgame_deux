import random
import time

# Farm animals for the matching game
farm_animals = ["🐄", "🐖", "🐑", "🐓", "🐐", "🦆", "🐕", "🐈"]
animals_pairs = farm_animals * 2  # Create pairs of animals
random.shuffle(animals_pairs)  # Shuffle the cards

# Initialize the game board
board = ["�"] * len(animals_pairs)  # "�" represents a face-down card
matched = [False] * len(animals_pairs)  # Track matched cards

def print_board():
    """Prints the current state of the board."""
    for i in range(0, len(board), 4):  # Display 4 cards per row
        row = board[i:i+4]
        print(" ".join(row))
    print()

def flip_card(index):
    """Flips a card to reveal the animal."""
    if board[index] == "�" and not matched[index]:
        board[index] = animals_pairs[index]
        return True
    return False

def check_match(index1, index2):
    """Checks if two cards match."""
    return animals_pairs[index1] == animals_pairs[index2]

def is_game_over():
    """Checks if all pairs have been matched."""
    return all(matched)

def play_game():
    """Main function to run the matching game."""
    print("Welcome to the Farm Animal Matching Game!")
    print("Match the pairs of farm animals by flipping two cards at a time.")
    print("Here's the board:")
    print_board()

    while not is_game_over():
        try:
            # Get the first card
            card1 = int(input("Enter the position of the first card (0-15): "))
            if card1 < 0 or card1 >= len(board):
                print("Invalid position. Please choose a number between 0 and 15.")
                continue
            if not flip_card(card1):
                print("This card is already flipped or matched. Try again.")
                continue

            print_board()

            # Get the second card
            card2 = int(input("Enter the position of the second card (0-15): "))
            if card2 < 0 or card2 >= len(board):
                print("Invalid position. Please choose a number between 0 and 15.")
                board[card1] = "�"  # Flip the first card back
                print_board()
                continue
            if not flip_card(card2):
                print("This card is already flipped or matched. Try again.")
                board[card1] = "�"  # Flip the first card back
                print_board()
                continue

            print_board()

            # Check if the cards match
            if check_match(card1, card2):
                print("Match found! 🎉")
                matched[card1] = True
                matched[card2] = True
            else:
                print("No match. Try again!")
                time.sleep(2)  # Pause to let the player see the cards
                board[card1] = "�"
                board[card2] = "�"
                print_board()

        except ValueError:
            print("Invalid input. Please enter a number.")

    print("Congratulations! You've matched all the pairs! 🏆")

# Run the game
if __name__ == "__main__":
    play_game()
