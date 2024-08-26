import pygame, random, math

pygame.init()

# Farbkodierung RGB

WHITE = (255, 255, 255)

BLACK = (0, 0, 0)

GRAY = (100, 100, 100)

# Zusätzliche Farbkodierung

RED = (255, 0, 0)

GREEN = (0, 255, 0)

BLUE = (0, 0, 255)

YELLOW = (255, 255, 0)

CYAN = (0, 255, 255)

MAGENTA = (255, 0, 255)

ORANGE = (255, 165, 0)

PURPLE = (128, 0, 128)

BROWN = (165, 42, 42)

PINK = (255, 192, 203)

LIME = (0, 255, 0)

TEAL = (0, 128, 128)

NAVY = (0, 0, 128)

OLIVE = (128, 128, 0)

MAROON = (128, 0, 0)

AQUA = (0, 255, 255)

CORAL = (255, 127, 80)

SALMON = (250, 128, 114)

CHOCOLATE = (210, 105, 30)

TURQUOISE = (64, 224, 208)

INDIGO = (75, 0, 130)

TAN = (210, 180, 140)

FUCHSIA = (255, 0, 255)

VIOLET = (238, 130, 238)

PERU = (205, 133, 63)

KHAKI = (240, 230, 140)

SILVER = (192, 192, 192)

PLUM = (221, 160, 221)

# Liste der verfügbaren Farben für die Objekte im Spiel

COLOR_LIST = [

    WHITE, RED, GREEN, BLUE, YELLOW, CYAN, MAGENTA, ORANGE, PURPLE, GRAY,

    BROWN, PINK, LIME, TEAL, NAVY, OLIVE, MAROON, AQUA, CORAL,

    SALMON, CHOCOLATE, TURQUOISE, INDIGO, TAN, FUCHSIA, VIOLET, PERU, KHAKI,

    SILVER, PLUM

]

# Fensterabmessungen

WIDTH = 800

HEIGHT = 600

screen = pygame.display.set_mode((WIDTH, HEIGHT))

pygame.display.set_caption("Pong")

# Schriftarten für verschiedene Bildschirmtexte

title_font = pygame.font.Font(None, 100)

button_font = pygame.font.Font(None, 74)

small_font = pygame.font.Font(None, 50)

# Laden der Soundeffekte

hit_sound = pygame.mixer.Sound("hit.wav")

score_sound = pygame.mixer.Sound("score.wav")

win_sound = pygame.mixer.Sound("win.wav")


# Schlägerklasse: Definiert das Verhalten und die Darstellung eines Schlägers

class Paddle:

    def __init__(self, x, y, width, height, speed, color):

        self.rect = pygame.Rect(x, y, width, height)

        self.speed = speed

        self.color = color

    # Bewegung des Schlägers basierend auf der Richtung (oben oder unten)

    def move(self, up=True):

        if up:

            self.rect.y -= self.speed

        else:

            self.rect.y += self.speed

        # Begrenzung der Bewegung innerhalb des Fensters

        self.rect.y = max(0, min(self.rect.y, HEIGHT - self.rect.height))

    # Zeichnet den Schläger auf den Bildschirm

    def draw(self):

        pygame.draw.rect(screen, self.color, self.rect)

    def ai_move(self, ball, ai_speed, ):

        # Bewegung des Schlägers durch KI-Logik basierend auf der Ballposition

        if self.rect.centery < ball.rect.centery:

            self.rect.y += ai_speed * 0.9

        elif self.rect.centery > ball.rect.centery:

            self.rect.y -= ai_speed * 0.9

        # Begrenzung der Bewegung innerhalb des Fensters

        self.rect.y = max(0, min(self.rect.y, HEIGHT - self.rect.height))


# Ball

class Ball:

    def __init__(self, x, y, radius, speed, color):

        self.rect = pygame.Rect(x - radius, y - radius, 2 * radius, 2 * radius)

        self.speed_x = speed

        self.speed_y = speed

        self.radius = radius

        self.color = color

        # Zufällige Startrichtung

        while True:

            angle = random.uniform(0, 2 * math.pi)  # Zufälliger Winkel in Radiant

            self.speed_x = speed * math.cos(angle)

            self.speed_y = speed * math.sin(angle)

            if abs(self.speed_x) > 0.5 * speed and abs(self.speed_y) > 0.5 * speed:
                break

        # Liste, die die letzten Positionen des Balls speichert

        self.trail = []

    def move(self):

        self.rect.x += self.speed_x

        self.rect.y += self.speed_y

        # Kollision mit Wänden

        if self.rect.top <= 0 or self.rect.bottom >= HEIGHT:
            self.speed_y *= -1

            hit_sound.play()

        # Position des Balls zu seinem Schweif hinzufügen

        self.trail.append(self.rect.center)

        if len(self.trail) > 25:  # Begrenze die Anzahl der Punkte im Schweif

            self.trail.pop(0)

    def draw(self):

        # Zeichne den Schweif des Balls

        for a, pos in enumerate(self.trail):
            # Berechne die Transparenz basierend auf der Position im Schweif

            alpha = int(255 * (a / len(self.trail)))

            trail_color = (*self.color, alpha) if len(self.color) == 3 else self.color

            # Erstelle eine transparente Oberfläche

            trail_surface = pygame.Surface((self.radius * 2, self.radius * 2), pygame.SRCALPHA)

            pygame.draw.circle(trail_surface, trail_color, (self.radius, self.radius), self.radius)

            screen.blit(trail_surface, (pos[0] - self.radius, pos[1] - self.radius))

        # Zeichne den Ball

        pygame.draw.circle(screen, WHITE, self.rect.center, self.rect.width // 2)


# Funktion für das Hauptmenü

def main_menu():
    title_text = title_font.render("Pong by Joel Laux", True, WHITE)

    option1_text = button_font.render("1: Spiele mit einem Freund", True, random.choice(COLOR_LIST))

    option2_text = button_font.render("2: Spiele gegen die KI", True, random.choice(COLOR_LIST))

    while True:

        for event in pygame.event.get():

            if event.type == pygame.QUIT:
                return None  # Spiel beenden

            if event.type == pygame.KEYDOWN:

                if event.key == pygame.K_1:
                    return "friend"  # Spiel gegen Freund starten

                if event.key == pygame.K_2:
                    return "ai"  # Spiel gegen KI starten

        screen.fill(BLACK)

        # Titel zeichnen

        screen.blit(title_text, (WIDTH // 2 - title_text.get_width() // 2, HEIGHT // 4))

        # Optionen zeichnen

        screen.blit(option1_text, (WIDTH // 2 - option1_text.get_width() // 2, HEIGHT // 2 - 50))

        screen.blit(option2_text, (WIDTH // 2 - option2_text.get_width() // 2, HEIGHT // 2 + 50))

        pygame.display.flip()


# Spielobjekte


paddle_left = Paddle(50, HEIGHT // 2 - 50, 10, 100, 5, random.choice(COLOR_LIST))

paddle_right = Paddle(WIDTH - 60, HEIGHT // 2 - 50, 10, 100, 4.5, random.choice(COLOR_LIST))

ball = Ball(WIDTH // 2, HEIGHT // 2, 10, 4, random.choice(COLOR_LIST))

# Punktestand

score_left = 0

score_right = 0

max_score = 10  # Spielende bei 10 Punkten

# Spiel-Loop

running = True

paused = False

game_over = False

winner = ""

blink = True

blink_timer = pygame.time.get_ticks()

# Variablen für Menüfrage und Countdown

ask_exit = False

exit_confirmation = False

countdown = 0

clock = pygame.time.Clock()

# Hauptmenü aufrufen und auf Spielstart warten

mode = main_menu()

if mode is None:
    running = False  # Wenn None zurückgegeben wird das Spiel beenden

while running:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:
            running = False

        if event.type == pygame.KEYDOWN:

            if event.key == pygame.K_SPACE and not game_over:
                paused = not paused

            if event.key == pygame.K_r and game_over:
                # Spiel zurücksetzen

                score_left = 0

                score_right = 0

                ball = Ball(WIDTH // 2, HEIGHT // 2, 10, 4, random.choice(COLOR_LIST))

                game_over = False

                winner = ""

            # Drücke M, um die Frage anzuzeigen, ob man das Spiel verlassen will

            if event.key == pygame.K_m:

                if game_over:

                    mode = main_menu()

                    if mode is None:

                        running = False

                    else:

                        score_left = 0

                        score_right = 0

                        ball = Ball(WIDTH // 2, HEIGHT // 2, 10, 4, random.choice(COLOR_LIST))

                        game_over = False

                        paused = False

                        ask_exit = False

                else:

                    ask_exit = True

                    paused = True

            # Drücke Y, um das Spiel zu verlassen und ins Hauptmenü zu gehen

            if ask_exit and event.key == pygame.K_y:

                mode = main_menu()

                if mode is None:

                    running = False

                else:

                    # Setze das Spiel neu, wenn man wieder aus dem Hauptmenü kommt

                    score_left = 0

                    score_right = 0

                    ball = Ball(WIDTH // 2, HEIGHT // 2, 10, 4, random.choice(COLOR_LIST))

                    game_over = False

                    paused = False

                    ask_exit = False

            # Drücke N, um den Countdown zu starten

            if ask_exit and event.key == pygame.K_n:
                exit_confirmation = False

                ask_exit = False

                countdown = 4

    # Countdown-Logik

    if countdown > 0:

        countdown -= 1 / 60  # Countdown zählt pro Frame herunter (1 Sekunde = 60 Frames)

        if countdown <= 1:
            paused = False  # Spiel fortsetzen

    if not paused and not game_over:

        # Eingabe

        keys = pygame.key.get_pressed()

        if keys[pygame.K_w]:
            paddle_left.move(up=True)

        if keys[pygame.K_s]:
            paddle_left.move(up=False)

        if mode == "friend":

            if keys[pygame.K_UP]:
                paddle_right.move(up=True)

            if keys[pygame.K_DOWN]:
                paddle_right.move(up=False)

        elif mode == "ai":

            paddle_right.ai_move(ball, ai_speed=paddle_right.speed)

        # Ballbewegung

        ball.move()

        # Kollision mit Schlägern

        if ball.rect.colliderect(paddle_left) or ball.rect.colliderect(paddle_right):
            ball.speed_x *= -1.1  # Ball wird schneller

            hit_sound.play()

        # Punktevergabe

        if ball.rect.left <= 0:
            score_right += 1

            score_sound.play()

            ball = Ball(WIDTH // 2, HEIGHT // 2, 10, 4, random.choice(COLOR_LIST))

        if ball.rect.right >= WIDTH:
            score_left += 1

            score_sound.play()

            ball = Ball(WIDTH // 2, HEIGHT // 2, 10, 4, random.choice(COLOR_LIST))

        # Spielende

        if score_left >= max_score:

            game_over = True

            winner = "Left player wins!"

            win_sound.play()

        elif score_right >= max_score:

            game_over = True

            winner = "Right player wins!"

            win_sound.play()

    # Hintergrund

    screen.fill(BLACK)

    # Netz in der Mitte zeichnen

    for a in range(10, HEIGHT, 20):
        pygame.draw.rect(screen, WHITE, (WIDTH // 2 - 2, a, 4, 10))

    paddle_left.draw()

    paddle_right.draw()

    ball.draw()

    # Punktestand anzeigen

    score_text = button_font.render(f"{score_left} - {score_right}", True, WHITE)

    screen.blit(score_text, (WIDTH // 2 - score_text.get_width() // 2, 10))

    # Frage anzeigen, ob man ins Hauptmenü möchte (über zwei Zeilen)

    if ask_exit:
        question_line_1 = button_font.render("Do you want to exit the game?", True, WHITE)

        question_line_2 = button_font.render("(Y/N)", True, WHITE)

        screen.blit(question_line_1,

                    (WIDTH // 2 - question_line_1.get_width() // 2, HEIGHT // 2 - question_line_1.get_height()))

        screen.blit(question_line_2, (WIDTH // 2 - question_line_2.get_width() // 2, HEIGHT // 2 + 10))

    # Countdown anzeigen

    if countdown > 1:
        countdown_text = button_font.render(f"Resuming in {int(countdown)}", True, WHITE)

        screen.blit(countdown_text, (WIDTH // 2 - countdown_text.get_width() // 2, HEIGHT // 2 + 50))

    # Spielende anzeigen und Text-Hintergrund invertieren lassen

    if game_over:

        current_time = pygame.time.get_ticks()

        if current_time - blink_timer > 500:  # Wechsel alle 500ms

            blink = not blink


            blink_timer = current_time

        if blink:

            game_over_text_color = BLACK

            game_over_bg_color = WHITE

        else:

            game_over_text_color = WHITE

            game_over_bg_color = BLACK

        game_over_text_1 = title_font.render("GAME OVER", True, game_over_text_color, game_over_bg_color)

        winner_text = title_font.render(winner, True, game_over_text_color, game_over_bg_color)

        # Neue Textobjekte für "(r) restart" und "(m) menu"

        restart_text = small_font.render("(R) Restart", True, WHITE)

        menu_text = small_font.render("(M) Menu", True, WHITE)

        # Positioniere die Texte auf dem Bildschirm

        screen.blit(game_over_text_1, (

            WIDTH // 2 - game_over_text_1.get_width() // 2, HEIGHT // 2 - game_over_text_1.get_height() // 2 - 50))

        screen.blit(winner_text,

                    (WIDTH // 2 - winner_text.get_width() // 2, HEIGHT // 2 - winner_text.get_height() // 2 + 50))

        # Füge die Texte unterhalb der Gewinneranzeige hinzu

        screen.blit(restart_text,

                    (WIDTH // 2 - restart_text.get_width() // 2, HEIGHT // 2 + winner_text.get_height() // 2 + 100))

        screen.blit(menu_text,

                    (WIDTH // 2 - menu_text.get_width() // 2, HEIGHT // 2 + winner_text.get_height() // 2 + 150))

    pygame.display.flip()

    clock.tick(60)  # 60 FPS

pygame.quit()
