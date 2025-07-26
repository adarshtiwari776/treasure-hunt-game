import tkinter as tk
import random

GRID_SIZE = 5
CELL_SIZE = 80
MAX_MOVES = 15

class TreasureHuntGame:
    def __init__(self, root):
        self.root = root
        self.root.title("🏴‍☠️ Treasure Hunt Game")
        self.canvas = tk.Canvas(root, width=GRID_SIZE*CELL_SIZE, height=GRID_SIZE*CELL_SIZE)
        self.canvas.pack()

        self.info_label = tk.Label(root, text="", font=("Arial", 14))
        self.info_label.pack()

        self.button_frame = tk.Frame(root)
        self.button_frame.pack(pady=10)

        self.create_buttons()
        self.reset_game()

    def create_buttons(self):
        directions = [("↑", self.move_north), ("↓", self.move_south),
                      ("←", self.move_west), ("→", self.move_east)]
        for text, command in directions:
            btn = tk.Button(self.button_frame, text=text, font=("Arial", 16), width=5, command=command)
            btn.pack(side=tk.LEFT, padx=5)

    def reset_game(self):
        self.moves = 0
        self.player_pos = [0, 0]
        self.treasure_pos = [random.randint(0, GRID_SIZE-1), random.randint(0, GRID_SIZE-1)]
        while self.treasure_pos == self.player_pos:
            self.treasure_pos = [random.randint(0, GRID_SIZE-1), random.randint(0, GRID_SIZE-1)]
        self.update_canvas()

    def update_canvas(self):
        self.canvas.delete("all")
        for i in range(GRID_SIZE):
            for j in range(GRID_SIZE):
                x0 = i * CELL_SIZE
                y0 = j * CELL_SIZE
                x1 = x0 + CELL_SIZE
                y1 = y0 + CELL_SIZE
                self.canvas.create_rectangle(x0, y0, x1, y1, fill="white", outline="black")

        # Draw player
        px, py = self.player_pos
        self.canvas.create_text(px * CELL_SIZE + CELL_SIZE//2, py * CELL_SIZE + CELL_SIZE//2,
                                text="🧍", font=("Arial", 28))

        self.info_label.config(text=f"Moves: {self.moves}/{MAX_MOVES}")

    def move_player(self, dx, dy):
        if self.moves >= MAX_MOVES:
            return

        new_x = self.player_pos[0] + dx
        new_y = self.player_pos[1] + dy

        if 0 <= new_x < GRID_SIZE and 0 <= new_y < GRID_SIZE:
            self.player_pos = [new_x, new_y]
            self.moves += 1
            self.update_canvas()

            if self.player_pos == self.treasure_pos:
                self.reveal_treasure(won=True)
            elif self
