# To Do
- I tried object detection with [SSD](https://github.com/hiraku00/ssd_keras_test) and [YOLO v3](https://github.com/hiraku00/yolo3_keras_test), but this time I will try object detection with M2Det
- I will run this time on Google Colaboratory

# Summary
1. Preparation of environment (run by Google Colaboratory)
2. Download model from Google Drive
3. Copy image files
4. Run the model
5. View results

# Operatiing Environment
- google colaboratory
- pytorch
- opencv
- tqdm
- addict

# 1. Preparation of environment (run by Google Colaboratory)
- Open Google Colaboratory and change "Runtime Type" from "Change Runtime Type" to "GPU"
<img width="511" alt="スクリーンショット 2019-12-15 18.54.06.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/67194/13cb43d6-0af0-060f-a591-d294aff9dea1.png">

- Then continue to do the following
    - Install various packages
    - Clone repository
    - Run shell

```python:
!pip install torch torchvision
!pip install opencv-python tqdm addict
!git clone https://github.com/qijiezhao/M2Det.git
%cd M2Det/
!sh make.sh
```

# 2. Download model from Google Drive
- The link of the learned model is listed in the README on [GitHub](https://github.com/qijiezhao/M2Det), so download it on the code from this Google Drive link.(https://drive.google.com/file/d/1NM1UDdZnwHwiNDxhcP-nndaWj24m-90L/view)
- Download is based on this code(https://github.com/nsadawi/Download-Large-File-From-Google-Drive-Using-Python)
- You can download by command by executing `download_file_from_google_drive`

```python:
import requests

def download_file_from_google_drive(id, destination):
    URL = "https://docs.google.com/uc?export=download"

    session = requests.Session()

    response = session.get(URL, params = { 'id' : id }, stream = True)
    token = get_confirm_token(response)

    if token:
        params = { 'id' : id, 'confirm' : token }
        response = session.get(URL, params = params, stream = True)

    save_response_content(response, destination)    

def get_confirm_token(response):
    for key, value in response.cookies.items():
        if key.startswith('download_warning'):
            return value

    return None

def save_response_content(response, destination):
    CHUNK_SIZE = 32768

    with open(destination, "wb") as f:
        for chunk in response.iter_content(CHUNK_SIZE):
            if chunk: # filter out keep-alive new chunks
                f.write(chunk)

file_id = '1NM1UDdZnwHwiNDxhcP-nndaWj24m-90L'
destination = './m2det512_vgg.pth'
download_file_from_google_drive(file_id, destination)
```

# 3. Copy image files
- Mount Google Drive
- After that, copy all the jpg files in the folder containing the image files to "imgs" that runs M2Det
- In my environment, the image files are stored under My Drive > ML > work

```python:
from google.colab import drive
drive.mount('/content/drive')
```

```python:
!cp /content/drive/My\ Drive/ML/work/*.jpg ./imgs
```

# 4. Run the model
- Run the model

```python:
!python demo.py -c=configs/m2det512_vgg.py -m=m2det512_vgg.pth
```

# 5. View results
- Displays the run result
- The run image file is created like "XXX_m2det.jpg"

```python:
import cv2
import matplotlib.pyplot as plt

plt.figure(figsize=(5, 5), dpi=200)
img = cv2.imread('imgs/herd_of_horses_m2det.jpg')
show_img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
plt.imshow(show_img)
```

![ダウンロード1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/67194/1eb7f6a4-c967-c606-9672-f2265a8b63a9.png)

- Other run results

![ダウンロード2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/67194/876901ab-69d2-90c8-78fe-1f9bacac796e.png)

![ダウンロード3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/67194/dbe42c3e-da42-f22f-8606-ee8e7d7602a4.png)

![ダウンロード4.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/67194/cd612e05-f6d9-28d4-c752-045203b9d4fa.png)

# In Japanese
[Qiita](https://qiita.com/hiraku00/items/972ec178a6892ef77e63)
