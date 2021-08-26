# 預知保養（PdM）

## 專案介紹

工業製造業裡頭設備、機台的壽命會因使用週期而逐漸減低，為避免設備失效在不可預期的情況下，來減少停機時間，降低維護成本並提高運營效率和安全性。
「預知保養（PdM）」著重識別傳感器和產量資料中的固定模式，這些模式代表設備狀態的變化，通常是特定設備的磨損，它能預測故障，公司可以確定資產的剩餘價值，採取行動來維修、更換系統，甚至計劃何時出現故障。

### 專案動機：
* 利用LSTM長短期記憶網路，預測設備剩餘壽命(回歸問題)。
* 利用混合深度學習模型，結合CNN, LSTM, Bi-LSTM，預測設備剩餘壽命。

## 資料來源

- 本次資料是由NASA Ames 的 Prognostics CoE 提供引擎故障資料集，使用 C-MAPSS 進行發動機退化模擬，在不同的操作條件和故障模式組合下模擬了四種(FD001、FD002、FD003、FD004)不同的組。
- NASA生成的資料集可以來預測引擎執行一段時間後的故障，所有的引擎都是同一型別，但在製造過程中，每個引擎的初期的磨損程度和差異是不同的。
- 資料集包括21個引擎感測器的時間序列，時間序列在發生故障前的某個時間結束。

資料來源：https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/

## 專案步驟：

- 了解資料
- 資料前處理
- 模型評估
- 結論

## 了解資料

1. 訓練數據：是飛機發動機運行到故障的數據。
2. 測試數據：是沒有記錄故障事件的飛機發動機運行數據。
3. 地面實況數據：它包含測試數據中每個發動機的真實剩餘循環信息。

## 資料前處理
1. RUL (Remaining useful life)
2. MinMax Normalization
3. Window size : 50 cycles
4. 隨機種子：

<img width="832" alt="image" src="https://user-images.githubusercontent.com/81677812/128854605-5aacef92-fea0-439a-9a16-26d9542f1799.png">

## 模型評估

|      模型演算法    |    RMSE    |  
|:------------------:|:-------------:|
|        LSTM      |    20.18   |
| CNN+LSTM+Bi-LSTM |    17.08   |

### CNN+LSTM+Bi-LSTM

<img width="512" alt="image" src="https://user-images.githubusercontent.com/81677812/130894382-cfc0be39-a269-4637-8c62-4a5b2435ad84.png">

混合模型結合各自優點：
1. CNN：利用Convolution Layer保留重要特徵、MaxPooling取樣屎運算複雜度降低。
2. LSTM：處理長時間序列，避免梯度爆炸或消失。
3. Bi-LSTM：增加反向的LSTM，多考慮反向數據，增加數據前後的依賴性。

### 探討LSTM避免過擬和的不同處理方法：
1. EarlyStopping
   * patience：val_loss訓練多少個 epoch 執行週期沒改善就停止。 min_delta 小，patient 可以相對降低；反之，則 patient 加大
   * min_delta：評斷監控的數據是否有改善標準，唯有當數據變動幅度大於 min_delta 才算是有改善。
   * verbose：有 0 或 1 兩種設置。 0 是 silent 不會輸出任何的訊息， 1 的話會輸出一些 debug 用的訊息。 
   * mode：有 auto, min 和 max 三種設置選擇。用來設定監控的數據的改善方向，若希望監控的數據是越大越好，則設置為 max，如：acc；反之，若希望數據越小越好，則設定 min，如：loss。

<img width="810" alt="image" src="https://user-images.githubusercontent.com/81677812/130912260-59792811-ec9a-463a-9fa1-9bf548723d24.png">

2. L2正則化
 
