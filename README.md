# codsoft-ai-task2def print_board(board):
    for row in board:
        print(" | ".join(row))
        print("-" * 9)

def check_winner(board):
   
    for row in range(3):
        if board[row][0] == board[row][1] == board[row][2] != " ":
            return board[row][0]
    
    for col in range(3):
        if board[0][col] == board[1][col] == board[2][col] != " ":
            return board[0][col]
    
    if board[0][0] == board[1][1] == board[2][2] != " ":
        return board[0][0]
    
    if board[0][2] == board[1][1] == board[2][0] != " ":
        return board[0][2]
    
    return None

def is_draw(board):
    return all(cell != " " for row in board for cell in row) and check_winner(board) is None

def available_moves(board):
    return [(r, c) for r in range(3) for c in range(3) if board[r][c] == " "]

def minimax(board, depth, is_maximizing):
    winner = check_winner(board)
    if winner == "O":
        return 1
    elif winner == "X":
        return -1
    elif is_draw(board):
        return 0

    if is_maximizing:
        best_score = float('-inf')
        for (r, c) in available_moves(board):
            board[r][c] = "O"  
            score = minimax(board, depth + 1, False)
            board[r][c] = " "  
            best_score = max(score, best_score)
        return best_score
    else:
        best_score = float('inf')
        for (r, c) in available_moves(board):
            board[r][c] = "X" 
            score = minimax(board, depth + 1, True)
            board[r][c] = " " 
            best_score = min(score, best_score)
        return best_score

def get_best_move(board):
    best_move = None
    best_score = float('-inf')
    for (r, c) in available_moves(board):
        board[r][c] = "O"  
        score = minimax(board, 0, False)
        board[r][c] = " " 
        if score > best_score:
            best_score = score
            best_move = (r, c)
    return best_move

def tic_tac_toe():
    board = [[" " for _ in range(3)] for _ in range(3)]
    print("Welcome to Tic Tac Toe!")
    print("You are 'X' and the AI is 'O'. Make your move (0-8):")
    print("Board positions:")
    print("0 | 1 | 2")
    print("---------")
    print("3 | 4 | 5")
    print("---------")
    print("6 | 7 | 8")

    while True:
        print_board(board)
        
       
        player_move = int(input("Enter your move (0-8): "))
        row, col = player_move // 3, player_move % 3
        
        if (0 <= player_move <= 8) and board[row][col] == " ":
            board[row][col] = "X"  
        else:
            print("Invalid move, try again.")
            continue
        
        if check_winner(board):
            print_board(board)
            print("Congratulations! You win!")
            break
        
        if is_draw(board):
            print_board(board)
            print("It's a draw!")
            break
        
        
        ai_move = get_best_move(board)
        if ai_move:
            board[ai_move[0]][ai_move[1]] = "O"  
            if check_winner(board):
                print_board(board)
                print("AI wins! Better luck next time.")
                break
        
        if is_draw(board):
            print_board(board)
            print("It's a draw!")
            break


if _name_ == "_main_":
    tic_tac_toe()
