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
- CNN+LSTM+Bi-LSTM結構
- 探討LSTM避免Overfitting的不同處理方法
- 探討Batch size的影響

## 了解資料

1. 訓練數據：是飛機發動機運行到故障的數據。
2. 測試數據：是沒有記錄故障事件的飛機發動機運行數據。
3. 地面實況數據：它包含測試數據中每個發動機的真實剩餘循環信息。

## 資料前處理
1. RUL (Remaining useful life)
2. MinMax Normalization
3. Window size : 50 cycles

<img width="832" alt="image" src="https://user-images.githubusercontent.com/81677812/128854605-5aacef92-fea0-439a-9a16-26d9542f1799.png">

## 模型評估

|      模型演算法    |    RMSE    |  
|:------------------:|:-------------:|
|        LSTM      |    20.18   |
| CNN+LSTM+Bi-LSTM |    17.08   |

### CNN+LSTM+Bi-LSTM結構

<img width="512" alt="image" src="https://user-images.githubusercontent.com/81677812/130894382-cfc0be39-a269-4637-8c62-4a5b2435ad84.png">

混合模型結合各自優點：
1. CNN：利用Convolution Layer保留重要特徵、MaxPooling取樣屎運算複雜度降低。
2. LSTM：處理長時間序列，避免梯度爆炸或消失。
3. Bi-LSTM：增加反向的LSTM，多考慮反向數據，增加數據前後的依賴性。

### 探討LSTM避免過擬和的不同處理方法：
1. EarlyStopping
   * patience：val_loss訓練多少個 epoch 執行週期沒改善就停止。 min_delta 小，patient 可以相對降低；反之，則 patient 加大
   * min_delta：評斷監控的數據是否有改善標準，唯有當數據變動幅度大於 min_delta 才算是有改善。
   * mode：有 auto, min 和 max 三種設置選擇。用來設定監控的數據的改善方向，若希望監控的數據是越大越好，則設置為 max，如：acc；反之，若希望數據越小越好，則設定 min，如：loss。

<img width="802" alt="image" src="https://user-images.githubusercontent.com/81677812/131292260-c3f52f0f-d328-4370-85f9-0fde01370df6.png">


2. L2正則化

<img width="796" alt="image" src="https://user-images.githubusercontent.com/81677812/131351473-b85fdb6d-c4f8-4277-ae03-f8ef96a6ca5b.png">

<img width="793" alt="image" src="https://user-images.githubusercontent.com/81677812/131303089-c66ef2bd-c1e8-4016-9616-5ca403975a65.png">


### 探討Batch size的影響
batch_size表示單次訓練中，要投入多少資料給神經網路。為了讓梯度下降法有更好的效果：一個神經網路就像一條曲折的山稜線，梯度下降法就像是站在山稜線上的某一點，然後四下觀望後開始往下走一樣。
* 這個移動的過程分成兩個步驟：
1. 觀察四周狀況，決定移動方向。
2. 往前走一小步。

* 如果計算完所有的資料，才開始往下走，那會有兩個問題：
1. 要觀察很久才走一步，儘管這一步是最有效率的一步，但一小步還是一小步，所以還是會走得很慢。
2. 會很容易走進離你最近的局域最低點(Local Minimum)，然後就卡在那裏了。

### 所以設定 batch_size可以避免
因為使用batch_size的時候，只會觀察相當於batch_size數量的樣本並求其梯度的平均，也就是只看某些方向然後就開始移動，儘管還是會下降，但它移動的方向並不一定是最佳解，甚至可能會走錯。
可以想像設定batch_size後，移動就變成不由自主地橫衝直撞，雖然還是可以稍微控制要往哪裡移動，但大部分都會被逼著隨便亂跑，但看似沒效率的情況，反而避免了掉入局域最低點的情況。

<img width="780" alt="image" src="https://user-images.githubusercontent.com/81677812/131343637-8ac6977f-87fc-4495-9682-9c2e00f08e2c.png">

<img width="797" alt="image" src="https://user-images.githubusercontent.com/81677812/131343658-69c22c79-7151-4624-a052-91320ac58e04.png">
