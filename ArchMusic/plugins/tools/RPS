import logging
from telebot import TeleBot
import random
from config import BOT_TOKEN  # Import the token from the config file

# Enable logging
logging.basicConfig(level=logging.INFO)

# Define the game state
game_state = {'user_score': 0, 'bot_score': 0, 'target_score': random.randint(10, 20)}

# Define the bot
bot = TeleBot(BOT_TOKEN)

# Define the game logic
def start_game(message):
    global game_state
    game_state['user_score'] = 0
    game_state['bot_score'] = 0
    game_state['target_score'] = random.randint(10, 20)  # Random target score for the game
    bot.send_message(chat_id=message.chat.id, text=f'Game started! First to reach {game_state["target_score"]} points wins. Type "rock", "paper", or "scissors" to make a move.')

def make_move(message):
    global game_state
    user_choice = message.text.lower()
    bot_choice = random.choice(['rock', 'paper', 'scissors'])
    if user_choice not in ['rock', 'paper', 'scissors']:
        bot.send_message(chat_id=message.chat.id, text="Invalid move! Please choose 'rock', 'paper', or 'scissors'.")
        return
    
    # Generate random points for this round (e.g., 1 to 3 points)
    round_points = random.randint(1, 3)
    if user_choice == bot_choice:
        result = f"Tie! No points awarded."
    elif (user_choice == 'rock' and bot_choice == 'scissors') or \
         (user_choice == 'scissors' and bot_choice == 'paper') or \
         (user_choice == 'paper' and bot_choice == 'rock'):
        game_state['user_score'] += round_points
        result = f"You win this round and earn {round_points} points!"
    else:
        game_state['bot_score'] += round_points
        result = f"Bot wins this round and earns {round_points} points!"
    
    bot.send_message(chat_id=message.chat.id, text=f'You chose {user_choice}, bot chose {bot_choice}. {result}\nScore: You {game_state["user_score"]} - Bot {game_state["bot_score"]}')
    
    # Check if anyone has reached the target score
    if game_state['user_score'] >= game_state['target_score']:
        bot.send_message(chat_id=message.chat.id, text=f'Congratulations! You won the game with a score of {game_state["user_score"]} - {game_state["bot_score"]}.')
        ask_new_game(message)
    elif game_state['bot_score'] >= game_state['target_score']:
        bot.send_message(chat_id=message.chat.id, text=f'Sorry! The bot won the game with a score of {game_state["bot_score"]} - {game_state["user_score"]}.')
        ask_new_game(message)

def end_game(message):
    global game_state
    bot.send_message(chat_id=message.chat.id, text=f'Game over. Final score: You {game_state["user_score"]} - Bot {game_state["bot_score"]}')
    ask_new_game(message)

def ask_new_game(message):
    bot.send_message(chat_id=message.chat.id, text="Would you like to play a new game? Type '/start' to begin.")

# Send a greeting message when the bot starts
@bot.message_handler(commands=['rps'])
def handle_start(message):
    bot.send_message(chat_id=message.chat.id, text="Welcome to the Rock, Paper, Scissors game! You will be playing against the bot. Let's see who the winner will be!")
    start_game(message)

# Help command to explain the game rules
@bot.message_handler(commands=['help'])
def handle_help(message):
    help_text = (
        "This is a Rock, Paper, Scissors game where you play against the bot!\n\n"
        "Commands:\n"
        "/start - Start a new game\n"
        "/help - Show game instructions\n"
        "/end - End the current game\n\n"
        "Game Rules:\n"
        "1. Type 'rock', 'paper', or 'scissors' to make a move.\n"
        "2. Each round, you or the bot can win 1-3 points based on a random draw.\n"
        "3. The first to reach a randomly chosen target score wins the game!\n\n"
        "Good luck and have fun!"
    )
    bot.send_message(chat_id=message.chat.id, text=help_text)

# Define the handler for the player's moves
@bot.message_handler(func=lambda message: message.text.lower() in ['rock', 'paper', 'scissors'])
def handle_move(message):
    make_move(message)

# Define the handler for the end game command
@bot.message_handler(commands=['end'])
def handle_end(message):
    end_game(message)

# Start the bot
bot.infinity_polling()
