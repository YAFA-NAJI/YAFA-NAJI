import 'dart:io';

class TicTacToe {
  List<String> board = List.filled(9, ' ');
  bool isPlayerX = true; // Player X starts the game
  int moves = 0;

  void displayBoard() {
    print(' ${board[0]} | ${board[1]} | ${board[2]} ');
    print('-----------');
    print(' ${board[3]} | ${board[4]} | ${board[5]} ');
    print('-----------');
    print(' ${board[6]} | ${board[7]} | ${board[8]} ');
  }

  bool makeMove(int position) {
    if (position < 1 || position > 9 || board[position - 1] != ' ') {
      print('Invalid move. Please choose an empty cell (1-9).');
      return false;
    }
    board[position - 1] = isPlayerX ? 'X' : 'O';
    moves++;
    return true;
  }

  bool checkWinner() {
    List<List<int>> winningConditions = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ];

    for (var condition in winningConditions) {
      if (board[condition[0]] != ' ' &&
          board[condition[0]] == board[condition[1]] &&
          board[condition[1]] == board[condition[2]]) {
        return true;
      }
    }

    return false;
  }

  bool checkDraw() {
    return moves == 9;
  }

  void play() {
    print('Welcome to Tic-Tac-Toe!');

    do {
      displayBoard();
      int position;
      do {
        print('Player ${isPlayerX ? 'X' : 'O'}, enter your move (1-9):');
        position = int.tryParse(stdin.readLineSync()!);
      } while (!makeMove(position));

      if (checkWinner()) {
        displayBoard();
        print('Player ${isPlayerX ? 'X' : 'O'} wins!');
        break;
      } else if (checkDraw()) {
        displayBoard();
        print('It\'s a draw!');
        break;
      }

      isPlayerX = !isPlayerX; // Switch turns
    } while (true);

    print('Do you want to play again? (yes/no)');
    String playAgain = stdin.readLineSync()!.toLowerCase();
    if (playAgain == 'yes') {
      resetGame();
      play();
    } else {
      print('Thank you for playing Tic-Tac-Toe!');
    }
  }

  void resetGame() {
    board = List.filled(9, ' ');
    isPlayerX = true;
    moves = 0;
  }
}

void main() {
  TicTacToe game = TicTacToe();
  game.play();
}
