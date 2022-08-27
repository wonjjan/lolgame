from tkinter import *
import os
from random import randint as ri
from random import shuffle as sh
from  PIL import ImageTk
from tkinter import messagebox
from time import sleep
from time import time

tk = Tk()
tk.title("롤 같은그림 맞추기 게임")
tk.geometry("700x500")

## champs : 챔피언 파일 이름들의 리스트
champs = os.listdir("champs")
N = 4
def charchoice(x):
    s = set()
    while True:
        if len(s) == x ** 2 // 2:
            break
        s.add(ri(0, len(champs)-1))
    return list(s)

## lols : 같은 그림 맞추기로 뽑힌 캐릭터들의 인덱스 리스트(2개씩)
lols = charchoice(N) * 2
sh(lols)

#클릭한 버튼들의 인덱스들의 리스트
clicks = []
correct = []

def btnclick(x):
    global N
    if not x in clicks:
        clicks.append(x)
        button[x].image = photo[x]
        button[x]["image"] = photo[x]

    if len(clicks) == 2:
        if lols[clicks[0]] == lols[clicks[1]]:
            button[clicks[0]]["state"] = DISABLED
            button[clicks[1]]["state"] = DISABLED
            correct.append(clicks[1])
            if len(correct) == N**2:
                end = round(time()-start, 2)
                messagebox.showinfo("CLEAR",f"{end}초 걸림")
                exit()
        else:
            tk.update()
            sleep(1)
            button[clicks[0]].image = logo
            button[clicks[0]]["image"] = logo
            button[clicks[1]].image = logo
            button[clicks[1]]["image"] = logo
        clicks.clear()

logo = ImageTk.PhotoImage(file="logo.png")


# 클릭을 할떄마다 clicks 리스트에 추가가 되므로 clicks 리스트의 값을 없에면 된다
def 무작위한쌍():
    #기존에맞추지 않은 버튼 인댁스 뽑기
    while True:
        idx = ri(0,N**2-1)
        if not idx in correct:
            break
    b = []
    for i in range(N**2):
        if lols[idx] == lols[i]:
            b.append(i)
    
    for i in b:
        button[i].image = photo[i]
        button[i]["image"] = photo[i]

    tk.update()
    sleep(1)
    for i in b:
        button[1].image = logo
        button[i]["image"] = logo
    hint2["state"] = DISABLED

def 무작위3개():
    b = []
    while True:
        if len(lols) - len(correct) > 2:
            if len(set(b)) == 3:
                break
        else :
            if len(set(b)) == 2:
                break
        
        A = ri(0, N**2 -1)
        if not A in correct:
            b.append(A)

    for i in b:
        button[1].image = photo[i]
        button[i]["image"] = photo[i]
    tk.update()
    sleep(1)

    for i in b:
        button[1].image = logo
        button[i]["image"] = logo
    hint2["state"] = DISABLED

def 전체():
    b = []
    for i in range(N**2):
        if not i in correct:
            b.append(i)
    for i in b:
        button[1].image = photo[i]
        button[i]["image"] = photo[i]
    tk.update()
    sleep(0.5)

    for i in b:
        button[1].image = logo
        button[i]["image"] = logo
    hint3["state"] = DISABLED




button = [None] * (N**2)
photo = [None] * (N**2)
hint1 = Button(text="무작위 한쌍", width=10, font=("맑은고딕", 15, "bold"), command=무작위한쌍)
hint1.place(x=500,y=100)

hint2 = Button(text="무작위 세쌍", width=10, font=('맑은고딕', 15, "bold"), command=무작위3개)
hint2.place(x=500,y=150)

hint3 = Button(text="전체", width=10, font=('맑은고딕', 15, "bold"), command=전체)
hint3.place(x=500,y=250)

for i in range(N**2):
    photo[i] = ImageTk.PhotoImage(file=f"champs/{champs[lols[i]]}")
    button[i] = Button(image=photo[i], command=lambda x=i:btnclick(x))


## 버튼 배치관련
for i in range(N):
    for j in range(N):
        button[N*i+j].place(x=i*68, y=j*68)
a=1
messagebox.showinfo("GAME START", "게임이 시작됩니다")
start = time()
print(start)




for i in range(N**2):
    button[i].image = logo
    button[i]["image"] = logo

# set = 리스트의 중복 생략




tk.mainloop()


# 이번 코딩은 여러가지 복잡한 것들이 많아서 많이 힘들다
# 하지만 이러한 결과물을 봤을때 너무나도 재미있다





