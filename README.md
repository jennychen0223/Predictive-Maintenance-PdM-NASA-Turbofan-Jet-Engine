# NASA渦輪引擎 工業4.0預測性維護（PdM）

## 專案介紹

工業4.0使製造業變得越來越「聰明」，設備、機台的壽命會因使用週期而逐漸減低，為避免設備失效在不可預期的情況下，來減少停機時間，降低維護成本並提高運營效率和安全性。
「預測性維護（PdM）」著重識別傳感器和產量資料中的固定模式，這些模式代表設備狀態的變化，通常是特定設備的磨損，它能預測故障，公司可以確定資產的剩餘價值，採取行動來維修、更換系統，甚至計劃何時出現故障。

### 預測性維護可通過以下兩種方法之一來實現：
* 分類方法 – 預測在接下來的幾個週期（C/T）後，是否有可能發生故障(此處探討分類問題)。
* 迴歸方法 – 預測在下次故障發生之前的剩餘時間，稱之為剩餘使用壽命(RUL)。
* 分群方法 - 實際案例可能無Label(非監督/半監督)。

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
- 模型評估
- 結論

## 了解資料

1. 訓練數據：是飛機發動機運行到故障的數據。
2. 測試數據：是沒有記錄故障事件的飛機發動機運行數據。
3. 地面實況數據：它包含測試數據中每個發動機的真實剩餘循環信息。

## 資料前處理
1. 將週期(C/T)與剩餘壽命合併成TTF(Time to Failure)。
2. 設定閥值：可依需求訂定剩餘多少週期將視為失效，此處設定為C/T=30。
3. Label：將大於閥值標記為可使用(1)、低於閥值標記為已失效(0)。

<img width="832" alt="image" src="https://user-images.githubusercontent.com/81677812/128854605-5aacef92-fea0-439a-9a16-26d9542f1799.png">

## 模型評估

|  模型演算法 |  Accuracy  |  Precision  |  Recall  | F1 score |
|-----------|------------|-------------|----------|-----------|
|    LSTM   |    99.4%   |    95.5%    |  97.9%   |  96.8%    |
|    DNN    |    97.3%   |    95.2     |  97.2%   |  96.0%    |
|    SVC    |    91.1%   |    92.7%    |  91.0%   |  91.7%    |

* LSTM

<img width="419" alt="LSTM-4" src="https://user-images.githubusercontent.com/81677812/128854467-9dc0d21a-c019-4203-8b7c-fc1428e3e0c1.png">

* DNN

<img width="417" alt="DNN-3" src="https://user-images.githubusercontent.com/81677812/128854498-f1160a97-32ca-4848-80e9-1019176756ff.png">


## 結論
使用神經網路效果皆顯著不錯。而LSTM為一種遞迴神經網路，其原理將神經元輸出再接回神經元的輸入，使其具有長短期記憶的特性，因此資料含有週期與壽命數據，當有時間序列資料集可以利用LSTM、RNN等時間循環網路。



