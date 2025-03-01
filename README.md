# projeto-2
class ChessGame:
    def __init__(self):
        self.board = self.create_board()
    
    def create_board(self):
        board = [
            ["r", "n", "b", "q", "k", "b", "n", "r"],
            ["p", "p", "p", "p", "p", "p", "p", "p"],
            [" "] * 8,
            [" "] * 8,
            [" "] * 8,
            [" "] * 8,
            ["P", "P", "P", "P", "P", "P", "P", "P"],
            ["R", "N", "B", "Q", "K", "B", "N", "R"]
        ]
        return board
    
    def print_board(self):
        for row in self.board:
            print(" ".join(row))
        print()
    
    def is_valid_move(self, start, end):
        x1, y1 = start
        x2, y2 = end
        piece = self.board[x1][y1]

        if piece == " ":
            return False
        
        if piece.lower() == "p":  # Peão
            direction = -1 if piece.isupper() else 1
            if x2 == x1 + direction and y1 == y2 and self.board[x2][y2] == " ":
                return True
            if x2 == x1 + direction and abs(y2 - y1) == 1 and self.board[x2][y2] != " ":
                return True
        
        elif piece.lower() == "r":  # Torre
            if x1 == x2 or y1 == y2:
                return self.clear_path(start, end)
        
        elif piece.lower() == "b":  # Bispo
            if abs(x2 - x1) == abs(y2 - y1):
                return self.clear_path(start, end)
        
        elif piece.lower() == "q":  # Rainha
            if x1 == x2 or y1 == y2 or abs(x2 - x1) == abs(y2 - y1):
                return self.clear_path(start, end)
        
        elif piece.lower() == "n":  # Cavalo
            if (abs(x2 - x1), abs(y2 - y1)) in [(2, 1), (1, 2)]:
                return True
        
        elif piece.lower() == "k":  # Rei
            if max(abs(x2 - x1), abs(y2 - y1)) == 1:
                return True
        
        return False
    
    def clear_path(self, start, end):
        x1, y1 = start
        x2, y2 = end
        dx = (x2 - x1) // max(1, abs(x2 - x1)) if x1 != x2 else 0
        dy = (y2 - y1) // max(1, abs(y2 - y1)) if y1 != y2 else 0
        
        x, y = x1 + dx, y1 + dy
        while (x, y) != (x2, y2):
            if self.board[x][y] != " ":
                return False
            x += dx
            y += dy
        return True

# Exemplo de uso
game = ChessGame()
game.print_board()
print(game.is_valid_move((1, 0), (2, 0)))  # Movimento válido de peão
print(game.is_valid_move((0, 0), (0, 5)))  # Movimento inválido de torre por obstrução
