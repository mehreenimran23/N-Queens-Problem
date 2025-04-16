# Python3 implementation of the N-Queens-Problem
from random import randint

N = 8

# A utility function that configures the 2D array "board" and array "state" randomly to provide a starting point for the algorithm.
def configureRandomly(board, state):

	# Iterating through the column indices
	for i in range(N):

		# Getting a random row index
		state[i] = randint(0, 100000) % N;

		# Placing a queen on the obtained place in chessboard.
		board[state[i]][i] = 1;

# A utility function that prints the 2D array "board".
def printBoard(board):

	for i in range(N):
		print(*board[i])

# A utility function that prints the array "state".
def printState( state):
	print(*state)

# A utility function that compares two arrays, state1 and state2 and returns True if equal and False otherwise.
def compareStates(state1, state2):


	for i in range(N):
		if (state1[i] != state2[i]):
			return False;

	return True;

# A utility function that fills the 2D array "board" with values "value"
def fill(board, value):

	for i in range(N):
		for j in range(N):
			board[i][j] = value;


# This function calculates the objective value of the state(queens attacking each other) using the board by the following logic.
def calculateObjective( board, state):

	# For each queen in a column, we check for other queens falling in the line of our current queen and if found,
	# any, then we increment the variable attacking count. Number of queens attacking each other, initially zero.
	attacking = 0;

	# Variables to index a particular
	# row and column on board.
	for i in range(N):

		# At each column 'i', the queen is placed at row 'state[i]', by the definition of our state.

		# To the left of same row(row remains constant and col decreases)
		row = state[i]
		col = i - 1;
		while (col >= 0 and board[row][col] != 1) :
			col -= 1

		if (col >= 0 and board[row][col] == 1) :
			attacking += 1;

		# To the right of same row (row remains constant and col increases)
		row = state[i]
		col = i + 1;
		while (col < N and board[row][col] != 1):
			col += 1;

		if (col < N and board[row][col] == 1) :
			attacking += 1;

		# Diagonally to the left up (row and col simultaneously decrease)
		row = state[i] - 1
		col = i - 1;
		while (col >= 0 and row >= 0 and board[row][col] != 1) :
			col-= 1;
			row-= 1;

		if (col >= 0 and row >= 0 and board[row][col] == 1) :
			attacking+= 1;

		# Diagonally to the right down (row and col simultaneously increase)
		row = state[i] + 1
		col = i + 1;
		while (col < N and row < N and board[row][col] != 1) :
			col+= 1;
			row+= 1;

		if (col < N and row < N and board[row][col] == 1) :
			attacking += 1;

		# Diagonally to the left down (col decreases and row increases)
		row = state[i] + 1
		col = i - 1;
		while (col >= 0 and row < N and board[row][col] != 1) :
			col -= 1;
			row += 1;

		if (col >= 0 and row < N and board[row][col] == 1) :
			attacking += 1;

		# Diagonally to the right up (col increases and row decreases)
		row = state[i] - 1
		col = i + 1;
		while (col < N and row >= 0 and board[row][col] != 1) :
			col += 1;
			row -= 1;

		if (col < N and row >= 0 and board[row][col] == 1) :
			attacking += 1;

	# Return pairs.
	return int(attacking / 2);

# A utility function that generates a board configuration given the state.
def generateBoard(state):
    board = [[0] * N for _ in range(N)]
    for i in range(N):
        board[state[i]][i] = 1
    return board

# A utility function that copies contents of state2 to state1.
def copyState( state1, state2):

	for i in range(N):
		state1[i] = state2[i];

def getNeighbour(state):
 bs = [0] * N
 copyState(bs, state)
 minAttacks = calculateObjective(generateBoard(state), state)

 for c in range(N):
  row = state[c]
  for r in range(N):
      if r == row:
         continue

      tempstate = [0] * N
      copyState(tempstate, state)
      tempstate[c] = r
      tempAttacks = calculateObjective(generateBoard(tempstate), tempstate)

      if tempAttacks < minAttacks:
         minAttacks = tempAttacks
         copyState(bs, tempstate)

 return bs, minAttacks

def hillClimbing(state):
 while True:
  bs, minAttacks = getNeighbour(state)

  if minAttacks >= calculateObjective(generateBoard(state), state):
   return state
  copyState(state, bs)

# Driver code
state = [randint(0, N - 1) for _ in range(N)]
board = generateBoard(state)

# Perform hill climbing on the generated board
final_state = hillClimbing(state)
final_board = generateBoard(final_state)

print("Final State:")
printState(final_state)
print("Final Board:")
printBoard(final_board)
print("Objective Value:", calculateObjective(final_board, final_state))
