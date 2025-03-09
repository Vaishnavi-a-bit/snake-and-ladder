import random
import time
from typing import Dict, List, Tuple


class SnakesAndLadders:

    def __init__(self):
        # Game board size
        self.board_size = 100

        # Define snakes (head: tail)
        self.snakes = {
            98: 78,
            95: 75,
            93: 73,
            87: 24,
            64: 60,
            62: 19,
            56: 53,
            49: 11,
            48: 26,
            16: 6
        }

        # Define ladders (bottom: top)
        self.ladders = {
            1: 38,
            4: 14,
            9: 31,
            21: 42,
            28: 84,
            36: 44,
            51: 67,
            71: 91,
            80: 100
        }

        # Initialize player positions
        self.player_pos = 0
        self.computer_pos = 0
        self.game_over = False

    def roll_dice(self) -> int:
        """Roll a six-sided dice"""
        return random.randint(1, 6)

    def check_win(self, position: int) -> bool:
        """Check if the position has reached or exceeded the winning square"""
        return position >= self.board_size

    def get_final_position(self, position: int) -> int:
        """Calculate final position after considering snakes and ladders"""
        if position in self.snakes:
            print(
                f"Oops! Snake at {position}! Going down to {self.snakes[position]}"
            )
            return self.snakes[position]
        elif position in self.ladders:
            print(
                f"Yay! Ladder at {position}! Going up to {self.ladders[position]}"
            )
            return self.ladders[position]
        return position

    def make_move(self, current_pos: int, roll: int) -> int:
        """Make a move and return the new position"""
        new_pos = current_pos + roll

        # Check if exceeded board size
        if new_pos > self.board_size:
            print(f"Exceeded board size. Staying at {current_pos}")
            return current_pos

        # Get final position after snakes and ladders
        final_pos = self.get_final_position(new_pos)
        return final_pos

    def computer_strategy(self) -> None:
        """Computer's turn with strategic thinking"""
        print("\nComputer's turn...")
        time.sleep(1)  # Add delay for better game feel

        roll = self.roll_dice()
        print(f"Computer rolled: {roll}")

        new_pos = self.make_move(self.computer_pos, roll)
        self.computer_pos = new_pos
        print(f"Computer moved to position: {self.computer_pos}")

        if self.check_win(self.computer_pos):
            print("\nComputer wins!")
            self.game_over = True

    def player_turn(self) -> None:
        """Handle player's turn"""
        print("\nYour turn!")
        input("Press Enter to roll the dice...")

        roll = self.roll_dice()
        print(f"You rolled: {roll}")

        new_pos = self.make_move(self.player_pos, roll)
        self.player_pos = new_pos
        print(f"You moved to position: {self.player_pos}")

        if self.check_win(self.player_pos):
            print("\nCongratulations! You win!")
            self.game_over = True

    def display_board(self) -> None:
        """Display the current game state"""
        print("\nCurrent Positions:")
        print(f"You: {self.player_pos}")
        print(f"Computer: {self.computer_pos}")
        print("-" * 40)

    def play(self) -> None:
        """Main game loop"""
        print("Welcome to Snakes and Ladders!")
        print("Get to position 100 to win!")
        print("\nSnakes:", self.snakes)
        print("Ladders:", self.ladders)

        while not self.game_over:
            self.display_board()

            # Player's turn
            self.player_turn()
            if self.game_over:
                break

            # Computer's turn
            self.computer_strategy()
            if self.game_over:
                break

        # Final board state
        self.display_board()
        print("\nGame Over!")

        play_again = input("\nWould you like to play again? (yes/no): ")
        if play_again.lower().startswith('y'):
            self.__init__()
            self.play()
        else:
            print("Thanks for playing!")


if __name__ == "__main__":
    game = SnakesAndLadders()
    game.play()
