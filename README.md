# NASA-Turbofan-Jet-Engine_Predictive-Maintenance

## 專案介紹

工業4.0製造使工廠變得越來越「聰明」，設備、機台的壽命會因使用週期而逐漸減低，為避免設備失效在不可預期的情況下，來減少停機時間，降低維護成本並提高運營效率和安全性。
「預測性維護（PdM）」著重識別傳感器和產量資料中的固定模式，這些模式代表設備狀態的變化，通常是特定設備的磨損，它能預測故障，公司可以確定資產的剩餘價值，採取行動來維修、更換系統，甚至計劃何時出現故障。

### 預測性維護可通過以下兩種方法之一來實現：
* 分類方法 – 預測在接下來的幾個週期（C/T）後，是否有可能發生故障。
* 迴歸方法 – 預測在下次故障發生之前的剩餘時間，稱之為剩餘使用壽命（RUL）。

Turbofan引擎是一種現代的汽油渦輪引擎，NASA空間探索局用的就是這種引擎。NASA生成了下面的資料集來預測引擎執行一段時間後的故障。
所有的引擎都是同一型別，但在製造過程中，每個引擎的初期的磨損程度和差異是不同的，這一點使用者是不知道的。

## 資料來源

- 本次資料是由NASA Ames 的 Prognostics CoE 提供引擎故障資料集，使用 C-MAPSS 進行發動機退化模擬，在不同的操作條件和故障模式組合下模擬了四種(FD001、FD002、FD003、FD004)不同的組。
- NASA生成的資料集可以來預測引擎執行一段時間後的故障，所有的引擎都是同一型別，但在製造過程中，每個引擎的初期的磨損程度和差異是不同的。
- 資料集包括21個引擎感測器的時間序列，時間序列在發生故障前的某個時間結束

資料來源：https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/

## 專案步驟：

- 了解資料
- 資料前處理
- 演算法挑選
- 模型評估
- 預測

## 了解資料

1. 訓練數據：是飛機發動機運行到故障的數據。
2. 測試數據：是沒有記錄故障事件的飛機發動機運行數據。
3. 地面實況數據：它包含測試數據中每個發動機的真實剩餘循環信息。

## 資料前處理

1. 處理空值：刪除空值欄位效果較好
2. 類別型資料：利用Label Encoder轉換成數值資料
3. 數值型資料：分佈較離散，進行標準化
4. Age欄位：偏左分佈，故取Log使資料分布呈常態分佈

## 資料比較

1. 數據樣本中，因目標正、負資料比例分佈不平均，正：76.3%、負：23.7%，故採SMOTE方法在少數類樣本之間，利用差值來產生額外的樣本。
2. 利用特徵值比較，刪除關聯性較低欄位
3. 拆分訓練資料80%、測試資料20%

## 模型評估

|         模型結構          |  準確率 |
| -------------------------|--------| 
|    Logistic Regression   | 85.04% |
| Decision Tree Classifier | 86.59% |
| Random Forest Classifier | 88.93% |
|          XGBoost         | 90.05% |
|          Lightgbm        | 88.63% |
|            KNN           | 84.69% |
|            DNN           | 86.00% |

## LINE Bot實現
1. 建立聊天機器人，使用者依照機器人的指令，回傳客戶資料，最終會回傳預測結果
2. 優點：使用便利、快速回覆、可立即根據結果做相對應的處理

<img width="347" alt="image" src="https://user-images.githubusercontent.com/81677812/128297963-bb8d6dc5-05b1-4990-beb9-788243c86ab3.png">
<img width="341" alt="image" src="https://user-images.githubusercontent.com/81677812/128297888-e221daba-e9c8-4c03-9385-032177c85b61.png">


