##  嵌入式系統 - 實作9: AI起手式之圖像分類實作(W15)
Lab 9-1 AI應用起手式: 使用 TensorFlow Hub 進行圖像分類

![image](https://user-images.githubusercontent.com/89329457/143729033-ba04f508-6c09-4f57-8d12-658f260f115b.png)

![image](https://user-images.githubusercontent.com/89329457/143729315-c3771a26-3c04-4775-8b81-06526100078f.png)


程式

# 801 1. 載入必要的Module
import tensorflow as tf
import tensorflow_hub as hub

import requests
from PIL import Image
from io import BytesIO

import matplotlib.pyplot as plt
import numpy as np

from datetime import date
today = date.today()
ts3 = 'TA Grace'

## 803, Select an Image Classification model

image_size = 224
dynamic_size = False

model_name = "efficientnetv2-s" 

model_handle_map = {"efficientnetv2-s": "https://tfhub.dev/google/imagenet/efficientnet_v2_imagenet1k_s/classification/2"}

model_image_size_map = {"efficientnetv2-s": 384}

model_handle = model_handle_map[model_name]

print(f"Selected model: {model_name} : {model_handle}")

max_dynamic_size = 512
if model_name in model_image_size_map:
  image_size = model_image_size_map[model_name]
  dynamic_size = False
  print(f"Images will be converted to {image_size}x{image_size}")
else:
  dynamic_size = True
  print(f"Images will be capped to a max size of {max_dynamic_size}x{max_dynamic_size}")

labels_file = "https://storage.googleapis.com/download.tensorflow.org/data/ImageNetLabels.txt"

#download labels and creates a maps
downloaded_file = tf.keras.utils.get_file("labels.txt", origin=labels_file)

classes = []
i = 0
with open(downloaded_file) as f:
  labels = f.readlines()
  classes = [l.strip() for l in labels[1:]]
  i += 1
## 804 選項: ['tiger', 'bus', 'car', 'cat', 'dog', 'apple', 'turtle', 'flamingo', 'piano', 'honeycomb', 'teapot']
"""## 3. 選擇圖像: 您可以選擇以下圖像之一，或使用您自己的圖像。 請記住，模型的輸入大小各不相同，其中一些使用動態輸入大小（啟用對未縮放圖像的推斷）。 鑑於此，方法 load_image 已經將圖像重新縮放為預期格式。

選項: ['老虎'，'公共汽車'，'汽車'，'貓'，'狗'，'蘋果'，'烏龜'，'火烈鳥'，'鋼琴'，'蜂窩'，'茶壺']
param ['tiger', 'bus', 'car', 'cat', 'dog', 'apple', 'turtle', 'flamingo', 'piano', 'honeycomb', 'teapot']
"""
image_name ="fish"

images_for_test_map = {
    "tiger": "https://upload.wikimedia.org/wikipedia/commons/b/b0/Bengal_tiger_%28Panthera_tigris_tigris%29_female_3_crop.jpg",
    "bus": "https://upload.wikimedia.org/wikipedia/commons/6/63/LT_471_%28LTZ_1471%29_Arriva_London_New_Routemaster_%2819522859218%29.jpg",
    "car": "https://upload.wikimedia.org/wikipedia/commons/4/49/2013-2016_Toyota_Corolla_%28ZRE172R%29_SX_sedan_%282018-09-17%29_01.jpg",
    "cat": "https://upload.wikimedia.org/wikipedia/commons/4/4d/Cat_November_2010-1a.jpg",
    "dog": "https://upload.wikimedia.org/wikipedia/commons/archive/a/a9/20090914031557%21Saluki_dog_breed.jpg",
    "apple": "https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg",
    "turtle": "https://upload.wikimedia.org/wikipedia/commons/8/80/Turtle_golfina_escobilla_oaxaca_mexico_claudio_giovenzana_2010.jpg",
    "flamingo": "https://upload.wikimedia.org/wikipedia/commons/b/b8/James_Flamingos_MC.jpg",
    "piano": "https://upload.wikimedia.org/wikipedia/commons/d/da/Steinway_%26_Sons_upright_piano%2C_model_K-132%2C_manufactured_at_Steinway%27s_factory_in_Hamburg%2C_Germany.png",
    "honeycomb": "https://upload.wikimedia.org/wikipedia/commons/f/f7/Honey_comb.jpg",
    "teapot": "https://upload.wikimedia.org/wikipedia/commons/4/44/Black_tea_pot_cropped.jpg",
    "fish": "https://upload.wikimedia.org/wikipedia/commons/2/25/Herring2.jpg",
}

img_url = images_for_test_map[image_name]
image, original_image = load_image(img_url, image_size, dynamic_size, max_dynamic_size)
show_image(image, 'Scaled image')
## 805 加載 TensorFlow Hub!
classifier = hub.load(model_handle)

input_shape = image.shape
warmup_input = tf.random.uniform(input_shape, 0, 1.0)
## 806 在圖像上運行模型 (Run model on image)
# 
%time 
probabilities = tf.nn.softmax(classifier(image)).numpy()

top_5 = tf.argsort(probabilities, axis=-1, direction="DESCENDING")[0][:5].numpy()
np_classes = np.array(classes)
includes_background_class = probabilities.shape[1] == 1001

for i, item in enumerate(top_5):
  class_index = item if not includes_background_class else item - 1
  line = f'({i+1}) {class_index:4} - {classes[class_index]}: {probabilities[0][top_5][i]}'
  translation = classes[class_index]
  print(line, ', ', translation)

show_image(image, '')
CPU times: user 3 µs, sys: 1 µs, total: 4 µs
Wall time: 6.2 µs
(1)  948 - Granny Smith: 0.753545343875885 ,  Granny Smith
(2)  957 - pomegranate: 0.042439792305231094 ,  pomegranate
(3)  954 - banana: 0.024101482704281807 ,  banana
(4)  950 - orange: 0.011141256429255009 ,  orange
(5)  952 - fig: 0.008787382394075394 ,  fig

## 808 實作A:  Python自動翻譯功能Module測試
!pip install translate # 安裝翻譯模組
from translate import Translator
E_2_TW = Translator(to_lang="zh-TW") # 英翻中
print('完成安裝與載入翻譯模組 at %s.Thanks!' % today)

## Method 1 without loop
print('*** Method 1 without loop')
ts1, ts2 = 'computer', 'university'
translation1 = E_2_TW.translate(ts1)
print('1',ts1, translation1)
translation2 = E_2_TW.translate(ts2)
print('2',ts2, translation2)
translation3 = E_2_TW.translate(ts3) 
print('3',ts3, translation3)

## Method 2 with Loop
print('*** Method 2 with Loop')
explist = [ts1, ts2, ts3]
for i, ts in enumerate(explist):
  result = E_2_TW.translate(ts)
  print(i, ts, result)

print('*** Don
e by %s at ' % ts3,today, type(today))
# 809 實作B: 在圖像上運行模型
# 
%time 

probabilities = tf.nn.softmax(classifier(image)).numpy()

top_5 = tf.argsort(probabilities, axis=-1, direction="DESCENDING")[0][:5].numpy()
np_classes = np.array(classes)
includes_background_class = probabilities.shape[1] == 1001

for i, item in enumerate(top_5):
  class_index = item if not includes_background_class else item - 1
  line = f'({i+1}) {class_index:4} - {classes[class_index]}: {probabilities[0][top_5][i]}'
  translation1 = E_2_TW.translate(classes[class_index])
  print(line, ', ', translation1)

show_image(image, '')
"""實作C: 從已提供的選項中,找一張自己喜歡的照片來試試看"""

# Your Code

"""實作D (Optional): 從網路上找一張自己喜歡的照片來試試看 (jpg/png)"""

# Your Code
