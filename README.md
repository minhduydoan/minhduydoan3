import pygame
import random

# Khởi tạo Pygame
pygame.init()

# Các biến chính
WIDTH, HEIGHT = 800, 600
player_size = 50
player_x = WIDTH // 2 - player_size // 2
player_y = HEIGHT - player_size - 10

enemy_size = 50
enemy_speed = 5
enemies = []

# Tạo cửa sổ trò chơi
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Simple Avoidance Game")

# Hàm vẽ người chơi
def draw_player(x, y):
    pygame.draw.rect(screen, (0, 128, 255), [x, y, player_size, player_size])

# Hàm vẽ kẻ địch
def draw_enemy(x, y):
    pygame.draw.rect(screen, (255, 0, 0), [x, y, enemy_size, enemy_size])

# Hàm chạy trò chơi
def game_loop():
    clock = pygame.time.Clock()

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()

        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT] and player_x > 0:
            player_x -= 5
        if keys[pygame.K_RIGHT] and player_x < WIDTH - player_size:
            player_x += 5

        # Di chuyển kẻ địch và thêm mới nếu cần
        for enemy in enemies:
            enemy[1] += enemy_speed
            if enemy[1] > HEIGHT:
                enemy[0] = random.randint(0, WIDTH - enemy_size)
                enemy[1] = 0

        # Kiểm tra va chạm
        for enemy in enemies:
            if (
                player_x < enemy[0] + enemy_size
                and player_x + player_size > enemy[0]
                and player_y < enemy[1] + enemy_size
                and player_y + player_size > enemy[1]
            ):
                print("Game Over!")
                pygame.quit()
                quit()

        # Xóa màn hình
        screen.fill((255, 255, 255))

        # Vẽ người chơi và kẻ địch
        draw_player(player_x, player_y)
        for enemy in enemies:
            draw_enemy(enemy[0], enemy[1])

        # Thêm mới kẻ địch
        if random.randint(0, 100) < 5:
            enemies.append([random.randint(0, WIDTH - enemy_size), 0])

        # Cập nhật màn hình
        pygame.display.update()

        # Đặt FPS và cập nhật đồng hồ
        clock.tick(30)

# Bắt đầu trò chơi
game_loop()
import pygame
import time
import random

pygame.init()

# Định nghĩa màu sắc
white = (255, 255, 255)
yellow = (255, 255, 102)
black = (0, 0, 0)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)

# Khởi tạo cửa sổ trò chơi
dis_width = 600
dis_height = 400
dis = pygame.display.set_mode((dis_width, dis_height))
pygame.display.set_caption('Snake Game by OpenAI')

# Khởi tạo các tham số
snake_block = 10
snake_speed = 15

font_style = pygame.font.SysFont(None, 50)

# Hàm vẽ rắn
def our_snake(snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(dis, green, [x[0], x[1], snake_block, snake_block])

# Hàm vẽ thông điệp
def Your_score(score):
    value = font_style.render("Your Score: " + str(score), True, white)
    dis.blit(value, [0, 0])

# Hàm chạy trò chơi
def gameLoop():
    game_over = False
    game_close = False

    x1 = dis_width / 2
    y1 = dis_height / 2

    x1_change = 0
    y1_change = 0

    snake_List = []
    Length_of_snake = 1

    foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
    foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0

    while not game_over:

        while game_close == True:
            dis.fill(blue)
            Your_score(Length_of_snake - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0

        if x1 >= dis_width or x1 < 0 or y1 >= dis_height or y1 < 0:
            game_close = True
        x1 += x1_change
        y1 += y1_change
        dis.fill(blue)
        pygame.draw.rect(dis, red, [foodx, foody, snake_block, snake_block])
        snake_head = []
        snake_head.append(x1)
        snake_head.append(y1)
        snake_List.append(snake_head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]

        for x in snake_List[:-1]:
            if x == snake_head:
                game_close = True

        our_snake(snake_block, snake_List)
        Your_score(Length_of_snake - 1)

        pygame.display.update()

        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
            foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
            Length_of_snake += 1

        pygame.time.Clock().tick(snake_speed)

    pygame.quit()
    quit()

# Bắt đầu trò chơi
gameLoop()
