# AI CUP 2022 Identification of Spread through Air Spaces (STAS) in Pathology Images of Lung Adenocarcinoma (II): Using Image Segmentation Strategies to Recognize STAS Contour

<p> 針對肺腺癌 H&E 染色數位病理全切片影像，本競賽提供在腫瘤外的感興趣區域 (region of interest, ROI)以方框及不規則形狀像素層級之 STAS 標註資訊，運用影像分割作法於切割STAS輪廓。</p>

* TEAM_1277
* Private leaderboard：0.90546 / (Rank 13 / 307)

### Preprocessing (for cross validation)
+ 將資料分群讓不同特性的資料均勻分布各個 cross validation 的資料集 ， 以增加 ensemble 後的模型穩定度
  + way1 : 肉眼觀察分群 （資料集照一定順序分佈）
  + way2 : kmeans cluster 
    + step1 : Elbow Method 決定分4群最佳 (註: 用 keras inceptionV3 抽特徵)
    + step2 : 4 群均分到不同 training dataset
    ![Untitled (2)](https://user-images.githubusercontent.com/76427253/185758143-55d5da83-b20a-44a9-b1a4-ce580c15577a.png)
    ![Untitled (3)](https://user-images.githubusercontent.com/76427253/185758151-b167a888-fec3-4191-b528-abdcc8aa6626.png)

### Model Architecture (voting)
* 使用 segmentation_models_pytorch 所提供的 API 建立以下五個模型
  | Architectures | Encoders |
  | ------------- | -------- |
  | UnetPlusPlus  | Efficientnet b7 |
  | UnetPlusPlus  | Efficientnet b5 |
  | UnetPlusPlus  | SE-ResNeXt50-32x4d |
  | DeepLabV3Plus | Efficientnet b5 |
  | Linknet       | Efficientnet b5 |
* 最後將五個模型 output 做平均 (Average Voting)

### Postprocessing (blur + findcontours & fillpoly)
![image](https://user-images.githubusercontent.com/76427253/185757288-2752ff48-20bd-4841-aafa-0555585824ce.png)

## repo 解說
<p>前處理程式碼：Preprocessing.ipynb</p>
<p>訓練程式碼：Model.ipynb</p>
<p>辨識程式碼：Test.ipynb</p>
<p>模型檔案：https://drive.google.com/drive/folders/1Sq682KheFmneDXpLD5Y7cYXpxpHj-gup?usp=sharing</p>
<p>執行環境：TWCC</p>
<p>預測結果輸出：STAS.zip</p>
<p>執行環境：Pytorch 1.11.0</p>

## 檔案目錄結構
```
├── Preprocessing.ipynb               
├── Model.ipynb
├── Test.ipynb
├── Data
│   ├── SEG_Train_Datasets
│   │   ├── Train_Images
│   │   ├── Train_Annotations
│   │   ├── Train_Masks
│   │   ├── Fold1_Images
│   │   ├── Fold1_Masks
│   │   ├── Fold2_Images
│   │   ├── Fold2_Masks
│   │   ├── Fold3_Images
│   │   ├── Fold3_Masks
│   │   ├── Fold4_Images
│   │   ├── Fold4_Masks
│   │   ├── Test_Images
│   │   └──  Test_Masks
│   ├── Public_Image
│   └── Image   
├── model_weight                      
│   ├── best_model_1.pth
│   ├── best_model_2.pth               
│   ├── best_model_3.pth         
│   ├── best_model_4.pth               
│   └── best_model_5.pth
├── fig                      
│   ├── acc_1.png
│   ├── acc_2.png              
│   ├── acc_3.png         
│   ├── acc_4.png              
│   ├── acc_5.png
│   ├── loss_1.png
│   ├── loss_2.png              
│   ├── loss_3.png         
│   ├── loss_4.png              
│   └── loss_5.png
├── output                      
│   ├── Image           
│   └── Image_Postprocess
```
