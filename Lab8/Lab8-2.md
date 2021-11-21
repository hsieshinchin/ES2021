### ## Lab 8-2 零基礎Python快速入門與實作, 2/2, W14

![image](https://user-images.githubusercontent.com/89329457/142748991-e1b536a2-985d-49d6-a2d8-574898d2ea88.png)


``` python
實作8 Lab 8-3

# P1101
ts3 = 'TA Grace' # Please input your English name
!python --version #  Python 版本確認

# P1129
animals = ['cat', 'dog', 'monkey']
for animal in animals:
    print(animal)

# P1130
# [Python] 使用 enumerate() 函式來同時輸出索引與元素
# enumerate() 是 Python 當中經常會看到的函式，前者輸入一個可迭代的對象、比如說 List 資料型態；後者輸入開始的起點編號，為數字，若不設定時從 0 開始。
animals = ['cat', 'dog', 'monkey']
for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx, animal))

# P1131A
for idx, animal in enumerate(animals, start=1):
    print('#%d: %s' % (idx, animal))

for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx+1, animal))

# P1131B
# range語法架構：range(start, stop[, step])

for i in range(1,10,1):
  print(i,'*'*i)

for i in range(1,10):
  print(i,'+'*i)

# P1131C
for i in range(9,0,-1):
  print(i,'*'*i)

# P1131D, Python Nested Loops(巢狀迴圈), 簡單來說，就是迴圈中又有一層迴圈
# 我們來看一個範例: 5X5乘法表
for i in range(1,6,1):
  for j in range(1,6,1):
    print('%dX%d=%2d, ' % (i, j, i*j), end="")
  print('\n')

# P1132A
nums = [0, 1, 2, 3, 4]
squares = []
for x in nums:
    squares.append(x ** 2)
print(nums, '>> ', squares)

# P1132B
nums = [0, 1, 2, 3, 4]
squares = [x ** 2 for x in nums]
print(squares)

# P1132C
nums = [0, 1, 2, 3, 4]
even_squares = [x ** 2 for x in nums if x % 2 == 0]
print(even_squares)

# P1133A, Python While-Loops敘述: Python迴圈的另一種型式，與for-loop不一樣的地方是，while-loop是依據條件來重複執行運算

aa = 0

while aa < 10:
  print('aa=%d, aa+2=%d, aa*2=%d, aa-2=%d' % (aa, aa+2, aa*2, aa-2))
  aa+=1 # a=a+1

print('Done!')

# P1133B, break：直接中斷迴圈(e.g., while loop, for loop)，在break指令之後的運算皆不會執行

aa = 0

while aa < 10:
  if aa>5:
    print('*** %d > 5, so, bye-bye and exit the while loop!' % aa)
    break

  print('aa=%d, aa+2=%d, aa*2=%d, aa-2=%d' % (aa, aa+2, aa*2, aa-2))
  aa+=1

# P1133C, continue：同樣的在continue指令之後的運算不會執行，但是不會中斷迴圈，而是會繼續讀取下一個元素

aa = 0

while aa < 10:
  if aa>5:
    aa+=1 # for increase the number of aa
    continue
    print('*** %d > 5, so, bye-bye and exit the while loop!' % aa)
    
    
  print('aa=%d, aa+2=%d, aa*2=%d, aa-2=%d' % (aa, aa+2, aa*2, aa-2))
  aa+=1

# P1135
d = {'cat': 'cute', 'dog': 'furry', 'fish':'wet'}  # Create a new dictionary with some data
print(d['cat'])       # Get an entry from a dictionary; prints "cute"
print('cat' in d)     # Check if a dictionary has a given key; prints "True"


# P1136
d['fish'] = 'wet'    # Set an entry in a dictionary
print(d['fish'])      # Prints "wet"


# P1137
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal, legs in d.items():
    print('Method 1: A {} has {} legs'.format(animal, legs))
    print('Method 2: A %s has %d legs' % (animal, legs))
    print("\n")


# P1138
animals = {'cat', 'dog', 'fish'}
for idx, animal in enumerate(animals):
    print('#{}: {}'.format(idx + 1, animal))


# P1139
from math import sqrt
print({int(sqrt(x)) for x in range(30)})
print({sqrt(x) for x in range(30)})

# P1140
def sign(x):
    if x > 0:
        return 'positive'
    elif x < 0:
        return 'negative'
    else:
        return 'zero'

for x in [-1, 0, 1]:
    print(sign(x))

# P1141
def hello(name='Horace', loud=False):
    if loud:
        print('HELLO by True, %s' % name.upper())
    else:
        print('Hello by False, %s' % name)

hello()
hello('Bob')
hello('Fred', loud=True)

# P1142
class Car:

    # Constructor
    def __init__(self, name):
        self.name = name  # Create an instance variable

    # Instance method
    def greet(self, loud=False):
        if loud:
          print('HELLO, {}'.format(self.name.upper()))
        else:
          print('Hello, {}!'.format(self.name))

g = Car('Toyota')  
g.greet()        
g.greet(loud=True)

# P1144
# Compute the x and y coordinates for points on a sine curve
x = np.arange(0, 2 * np.pi, 0.01)
y = np.sin(x)

# Plot the points using matplotlib
plt.plot(x, y)

# P1145
y_sin = np.sin(x)
y_cos = np.cos(x)

# 使用 matplotlib 繪製點
plt.plot(x, y_sin)
plt.plot(x, y_cos)
plt.xlabel('x axis label')
plt.ylabel('y axis label')
plt.title('Sine and Cosine')
plt.legend(['Sine', 'Cosine'])

# P1146
# 計算 x 和 y 坐標
x = np.arange(0, 2 * np.pi, 0.01)
y_sin = np.sin(x)
y_cos = np.cos(x)

plt.subplot(211) # Laytout 2X1, plot #1
plt.plot(x, y_sin, color = 'r') # red
plt.title('sin-red')

plt.subplot(212)
plt.plot(x, y_cos, color = 'g') # green
plt.title('cos-green')

plt.show() # Show圖

# P1147
# 如何用 matplotlib 繪製一個圓？

x1 = y_cos 
x2 = y_sin 

fig, ax = plt.subplots(1)

ax.plot(x1, x2, color = 'r')
ax.set_aspect(1)

plt.xlim(-1.1,1.1)
plt.ylim(-1.1,1.1)

plt.grid(linestyle='--')
plt.title('Plot a circle by Horace', fontsize=10)
plt.savefig("plot_circle_matplotlib_01.png", bbox_inches='tight')

plt.show()

### 實作1185
from math import pi
x = np.arange(0, 2 * np.pi, 0.01)
y_sin = np.sin(x)
y_cos = np.cos(x)

y_sin2 = np.sin(x+pi)
y_cos2 = np.cos(x+pi)

plt.subplots_adjust( left=0.1, right=1.5, top=1.5, bottom=0.1, wspace=0.2, hspace=0.2)

plt.subplot(221)
plt.plot(x, y_sin, color ='r') # red
plt.title('sin_red by Grace')

plt.subplot(222)
plt.plot(x, y_cos, color ='g') # green
plt.title('cos_green by Grac')

plt.subplot(223)
plt.plot(x, y_sin2, color ='y') # yellow
plt.title('sin_yellow by Grace')

plt.subplot(224)
plt.plot(x, y_cos2, color ='b') # blue
plt.title('cos_blue by Grace')

plt.show()

### Final Result
from datetime import datetime
today = datetime.now()
print('*** Done by %s at ' % ts3,today, type(today))
```
