# bug-Gamelearn
一个有bug的数独，是自己的第二个练习
#gamelearn-2
import random#导入随机模块
#数独棋盘的全局变量eg
subo=[[0,0,0,0,0,0,0,0,0,0],
      [0,1,2,3,4,5,6,7,8,9],
      [0,4,5,6,7,8,9,1,2,3],
      [0,7,8,9,1,2,3,4,5,6],
      [0,2,3,4,5,6,7,8,9,1],
      [0,5,6,7,8,9,1,2,3,4],
      [0,8,9,1,2,3,4,5,6,7],
      [0,3,4,5,6,7,8,9,1,2],
      [0,6,7,8,9,1,2,3,4,5],
      [0,9,1,2,3,4,5,6,7,8],]
#建立储存所有待解决的空白方格的坐标变量
allPuzzle=[]
#判断游戏结束的变量
over=False
#简单难度为49个，困难难度为59个
EASY=49
HARD=59

def getpuzzle():

    global allPuzzle
    allPuzzle=[]
    for i in range(1,10):
          for j in range(1,10):
                if(subo[i][j]==0):
                      p=[i,j]
                      allPuzzle.append(p)

def validSubo():
      for i in range(1,10):
            li_row=[]
            li_col=[]
            for j in range(1,10):
                  if subo[i][j]!=0:
                        if subo[i][j]not in li_row:
                              li_row.append(subo[i][j])
                        else:
                              return False
                  if subo[i][j]!=0:
                        if subo[i][j]not in li_col:
                              li_col.append(subo[i][j])
                        else:
                              return False
#检查九宫格是否有重复值
      for i in range(1,10,3):
        for j in range(1,10,3):
              li_sq=[]
              for row in range(i,i+3):
                    for col in range(j,j+3):
                          if subo[row][col]!=0:
                                if subo[row][col]not in li_sq:
                                      li_sq.append(subo[row][col])
                                else:
                                      return False

        return True

def getAnwser(n):
      global sudo,allPuzzle,over
      if n==len(allPuzzle):
            over=True
            return True
      for i in range(1,10):
            sudo[allPuzzle[n][0]][allPuzzle[n][1]]=i
            if validSubo():
                  getAnwser(n+1)
            if over:
              return True
            sudo[allPuzzle[n][0]][allPuzzle[n][1]]=0
      return False

def printsudo():
      for i in range(1,10):
            print(sudo[i][1:10])

def shufflesudo():
      global sudo
      #数字交换
      #产生要替换的数字和被替换的数字
      source=random.randint(1,9)
      replace=random.randint(1,9)
      while source==replace:
            source=random.randint(1,9)
            replace=random.randint(1,9)
      #替换9×9的全部数字
      for i in range(1,10):
            for j in range(1,10):
                  if sudo[i][j]==source:
                     sudo[i][j]=replace
                  elif sudo[i][j]==replace:
                        sudo[i][j]=source
      #每一行和每一列的交换
      for i in range(5):
            base=random.choice([1,4,7])
            source=random.randint(base,base+2)
            replace=random.randint(base,base+2)
            #行交换
            sudo[source],sudo[replace]=sudo[replace],sudo[source]
            #列交换
            for k in range(1,10):
                  sudo[k][source],sudo[k][replace]=sudo[k][replace],sudo[k][source]
      #三行与三行交换，三列与三列交换
      source=random.choice([1,4,7])
      replace=random.choice([1,4,7])
      while source==replace:
            source=random.choice([1,4,7])
            replace=random.choice([1,4,7])
      for i in range(3):
            sudo[source+i],sudo[replace+i]=sudo[replace+i],sudo[source+i]
            for k in range(1,10):sudo[k][source+i],sudo[k][replace+i]=sudo[k][replace+i],sudo[k][source+i]

def generatesudo():
      global sudo
      sudo=[[0,0,0,0,0,0,0,0,0,0],
            [0,1,2,3,4,5,6,7,8,9],
            [0,4,5,6,7,8,9,1,2,3],
            [0,7,8,9,1,2,3,4,5,6],
            [0,2,3,4,5,6,7,8,9,1],
            [0,5,6,7,8,9,1,2,3,4],
            [0,8,9,1,2,3,4,5,6,7],
            [0,3,4,5,6,7,8,9,1,2],
            [0,6,7,8,9,1,2,3,4,5],
            [0,9,1,2,3,4,5,6,7,8],]
      nums=random.randint(10,15)
      #减少产生重复数独的处理方法
      for i in range(nums):
            shufflesudo()
i=-1
while i!=4:
      print('--------数独游戏--------')
      print('----写不出来的人是gay----')
      print('1:人机难度，你个杂口不会连这个都做不出来吧？')
      print('2:超难')
      print('3:输入你的答案,并与正确答案核对,(每个数字用空格隔开，空白数字用0代替)')
      print('4:退出')
      i = int(input('请输入数字:'))
      if i==1:
            print("人机难度的为")
            generatesudo()
            printsudo()
      elif i==2:
            print("超难")
            generatesudo()
            printsudo()
      elif i==3:
            for k in range(1,10):
                  sudo[k][1:10]=list(map(int,input().split()))
            print("答案如下")
            getpuzzle()
            getAnwser(0)
            printsudo()
