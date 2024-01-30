import pygame
import sys


class Paddle:
    def __init__(self, x, y, width, height, speed):
        self.rect = pygame.Rect(x, y, width, height)
        self.speed = speed

    def move(self, direction):
        self.rect.x += direction * self.speed


class Ball:
    def __init__(self, x, y, radius, speed_x, speed_y):
        self.rect = pygame.Rect(x - radius, y - radius, 2 * radius, 2 * radius)
        self.speed_x = speed_x
        self.speed_y = speed_y

    def move(self):
        self.rect.x += self.speed_x
        self.rect.y += self.speed_y


class Brick:
    def __init__(self, x, y, width, height):
        self.rect = pygame.Rect(x, y, width, height)


def show_win_screen():
    pygame.init()
    win_screen = pygame.display.set_mode((200, 100))
    pygame.display.set_caption("You Win!")
    font = pygame.font.Font(None, 36)
    text = font.render("You Win!", True, (255, 255, 255))
    win_screen.blit(text, (50, 30))
    pygame.display.flip()
    pygame.time.delay(5000)
    pygame.quit()


def show_game_over_screen():
    pygame.init()
    game_over_screen = pygame.display.set_mode((200, 100))
    pygame.display.set_caption("Game Over")
    font = pygame.font.Font(None, 36)
    text = font.render("Game Over", True, (255, 255, 255))
    game_over_screen.blit(text, (50, 30))
    pygame.display.flip()
    pygame.time.delay(5000)
    pygame.quit()
    sys.exit()


def main():
    # Other constant
    WIDTH, HEIGHT = 1500, 1000
    FPS = 110
    WHITE = (255, 255, 255)
    BLACK = (0, 0, 0)

    # Initialize the game window
    pygame.init()
    screen = pygame.display.set_mode((WIDTH, HEIGHT))
    pygame.display.set_caption("Breakout Game")
    clock = pygame.time.Clock()

    # Create game objects
    paddle = Paddle((WIDTH - 100) // 2, HEIGHT - 50, 100, 20, 15)  # Increased paddle speed
    ball = Ball(WIDTH // 2, HEIGHT // 2, 10, 5, 5)
    bricks = [Brick(col * 80, row * 40, 60, 30) for row in range(10) for col in range(WIDTH // 90)]

    # Game variables
    fall_count = 0
    score = 0

    # Game loop
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        # Paddle movement
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT] and paddle.rect.x > 0:
            paddle.move(-1)
        if keys[pygame.K_RIGHT] and paddle.rect.x < WIDTH - paddle.rect.width:
            paddle.move(1)

        # Ball movement
        ball.move()

        # Ball collisions with walls
        if ball.rect.left <= 0 or ball.rect.right >= WIDTH:
            ball.speed_x *= -1
        if ball.rect.top <= 0:
            ball.speed_y *= -1

        # Ball collisions with paddle
        if ball.rect.colliderect(paddle.rect):
            ball.speed_y *= -1

        # Ball collisions with bricks
        for brick in bricks:
            if ball.rect.colliderect(brick.rect):
                ball.speed_y *= -1
                bricks.remove(brick)
                score += 1  # Increase the score

        # Check if the ball hits the bottom
        if ball.rect.bottom >= HEIGHT:
            fall_count += 1
            ball.rect.x = WIDTH // 2
            ball.rect.y = HEIGHT // 2

        # Check if all bricks are destroyed
        if not bricks:
            show_win_screen()
            pygame.quit()
            sys.exit()

        # Check if the ball falls 10 times
        if fall_count >= 10:
            show_game_over_screen()

        # Drawing goes here
        screen.fill(BLACK)
        pygame.draw.rect(screen, WHITE, paddle.rect)
        pygame.draw.circle(screen, WHITE, ball.rect.center, ball.rect.width // 2)

        for brick in bricks:
            pygame.draw.rect(screen, WHITE, brick.rect)

        # Display score and fall count on the side of the screen
        font = pygame.font.Font(None, 36)
        score_text = font.render(f"Score: {score}", True, (0, 0, 255))  # Blue color
        fall_text = font.render(f"Falls: {fall_count}", True, (0, 0, 255))  # Blue color
        screen.blit(score_text, (10, 10))
        screen.blit(fall_text, (10, 50))

        pygame.display.flip()
        clock.tick(FPS)


if __name__ == "__main__":
    main()
