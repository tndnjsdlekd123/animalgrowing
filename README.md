# animalgrowing

import pygame as p
import os
import random
##############################################################
# 기본 초기화 (반드시 해야 하는 것들)
p.init()

# 화면 크기 설정
screen_width = 735 # 가로 크기
screen_height = 960 # 세로 크기
screen = p.display.set_mode((screen_width, screen_height))

# 화면 타이틀 설정
p.display.set_caption("똥 피하기")

# FpS
clock = p.time.Clock()
  ##############################################################

# 1. 사용자 게임 초기화 (배경 화면, 게임 이미지, 좌표, 속도, 폰트 등)
current_path = os.path.dirname(__file__)
image_path = os.path.join(current_path, "images")


font_path = os.path.join(current_path, "images","Ownglyph_2022_UWY_Yoon_Yeong-Rg.ttf")  # 폰트 파일 경로 설정
font = p.font.Font(font_path, 50)
font_small =p.font.Font(font_path, 40)

background = p.image.load(os.path.join(image_path, "background2.png"))


character = p.image.load(os.path.join(image_path, "인간1.png"))
character_size = character.get_rect().size
character_height = character_size[1]
character_width = character_size[0]
character_x_pos = (screen_width - character_width) / 2
character_y_pos = screen_height - character_height
   
d = p.image.load(os.path.join(image_path, "똥.png"))
d_size = d.get_rect().size
d_height = d_size[1]
d_width = d_size[0]
d_x_pos = 0
d_y_pos = 0


dd = p.image.load(os.path.join(image_path, "똥.png"))
dd_size = dd.get_rect().size
dd_height = dd_size[1]
dd_width = dd_size[0]
dd_x_pos = 110
dd_y_pos = 0

to_x = 0

#미니게임 점수 변수
score = 0
#font = p.font.load(os.path.join(current_path, "Ownglyph_2022_UWY_Yoon_Yeong-rg"), 36)

p.mixer.music.load(os.path.join(image_path, "mymistake.mp3"))
p.mixer.music.play(-1)

timer_interval = 100  # 100초
timer_event = p.USEREVENT + 1
p.time.set_timer(timer_event, timer_interval * 1000)  # 밀리초 단위로 설정

# 점수
score = 0

# 게임 루프
running = True
timer_running = True

running = True
running = True
to_x = 0  # 플레이어 이동 방향 초기화

while running:
    dt = clock.tick(60)
    
    d_y_pos += 14
    dd_y_pos += 9
    
    #p.time_sleep

    for event in p.event.get():
        if event.type == p.QUIT:
            running = False
        elif event.type == timer_event:
            timer_running = False
            p.time.set_timer(timer_event, 0)  # 타이머 종료
        
        # 키 이벤트 처리
        elif event.type == p.KEYDOWN: 
            if event.key == p.K_LEFT:
                to_x = -5
            elif event.key == p.K_RIGHT:
                to_x = 5
        elif event.type == p.KEYUP:
            if event.key == p.K_LEFT or event.key == p.K_RIGHT:
                to_x = 0
    
    # 타이머가 실행 중일 때
    if timer_running == True:
        # 남은 시간 계산
        remaining_time = max(0, (timer_interval * 1000 - p.time.get_ticks()) // 1000)
        timer_text = font.render(f"남은 시간: {remaining_time}초", True, (0, 0, 0))
        
    else:
        # 결과 창
        screen.fill((255, 255, 255))
        #timer_text = font.render(f"게임 종료 - 점수: {score}", True, (0, 0, 0))
    
    # 캐릭터 위치 업데이트
    character_x_pos += to_x

    if character_x_pos < 0:
        character_x_pos = 0 
    elif character_x_pos > screen_width - character_width:
        character_x_pos = screen_width - character_width
   # if d_y_pos == 0:


    if d_y_pos > screen_height - d_height:
        d_y_pos = 0
        d_x_pos = random.randint(0, screen_width - d_width)
        print(d_x_pos)

    if dd_y_pos > screen_height - dd_height:
        dd_y_pos = 0
        dd_x_pos = random.randint(0, screen_width - dd_width)
        print(dd_x_pos)

    
    
    

    

    
    
    # 4. 충돌 처리

    

    character_rect = character.get_rect()
    character_rect.top = character_y_pos
    character_rect.left = character_x_pos

    d_rect = character.get_rect()
    d_rect.top = d_y_pos
    d_rect.left = d_x_pos
    if character_rect.colliderect(d_rect):
        print("crashed")
        timer_text = font.render(f"게임 종료 - 점수: {score}", True, (0, 0, 0))
        running = False

    dd_rect = character.get_rect()
    dd_rect.top = dd_y_pos
    dd_rect.left = dd_x_pos
    if character_rect.colliderect(dd_rect):
        print("crashed")
        timer_text = font.render(f"게임 종료 - 점수: {score}", True, (0, 0, 0))
        running = False

    # 5. 화면에 그리기
    screen.blit(background, (0, 0))
    screen.blit(character, (character_x_pos, character_y_pos))
    screen.blit(d, (d_x_pos, d_y_pos))
    screen.blit(dd, (dd_x_pos, dd_y_pos))
    screen.blit(timer_text, (10, 10))
    p.display.update()

p.quit()
