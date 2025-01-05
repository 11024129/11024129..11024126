# **4+1視圖建模及架構設計工程實踐**
11024129蔡東峻 11024126吳建宏

佔春良：碧桂園服務技術專家，專案架構師，前阿里資深軟體工程師，12年技術開發經驗。
# 01 前言
架構設計建模的目的是透過統一的UML語言，完成業務的梳理，並對業務系統進行合理的組織（分層、分模組），以提高系統的可擴展性、可重用性、可移植性、易理解性和易測試性，從而達到一個高品質屬性的軟體系統。

架構設計的關鍵內容主要包含以下幾個面向：

劃分邏輯視圖：確定領域；

選擇架構風格：確定領域的架構風格，如單體或分散式；

梳理領域之間依賴關係，並進行適當抽象（隔離依賴）；

技術選型（可選）。

**前置條件**
：在進行架構設計之前，我們需要先整理清楚業務流程，並使用UML統一語言來呈現統一的組織價值目標。透過清晰的圖標，我們可以更好地傳達訊息，一圖勝過千文。
# 02 架構建模簡介
# 1、"4+1"視圖組織建模
本文主要介紹「4+1」視圖架構建模的過程，先來看看「4+1」視圖模型定義：
![image](https://github.com/11024129/11024129..11024126/blob/main/%E5%9C%96%E4%B8%80.jpg)
)
「4+1」視圖是描述邏輯架構的重要工具，最早由Philippe Kruchten提出。目前「4+1」視圖已成為主流的架構設計標準。本文中，我們將聚焦於用例視圖對業務流程的梳理（如何進行有效的組織業務建模）以及邏輯視圖、開發視圖的分層架構開發程式碼實務。

組織業務建模是關鍵的業務梳理環節，在「4+1」視圖中，使用案例視圖需要準確表達組織的業務用例和系統用例。因此，在開始之前，我們先對用例圖和UML的一些要點進行準備。
# 2、業務用例和系統用例的區分
# 2.1 業務用例
業務用例的定義是指業務執行者希望透過和所研究組織進行互動來獲得價值。在定義業務用例時，我們需要將視角放在組織外部和執行者身上，專注於他們希望從組織中獲得什麼價值，而不是僅僅關注組織能提供什麼（註：這個價值不是指組織能提供什麼，而是指執行者想要什麼）。

以小區物業的充電樁管理中心為例，其業務執行者主要是業主。儘管小區物業可以提供投訴等服務，但業主對充電樁管理中心最大的需求是充電服務，因此，「業主->充電」就是物業充電樁管理中心的業務用例，提供充電服務則是該組織的最大價值所在。
![image](https://github.com/11024129/11024129..11024126/blob/main/%E5%9C%96%E4%B8%80.drawio.png)
# 2.2 系統用例
系統用例的定義是指系統能夠為執行者提供的、涉眾可以接受的價值。為了解釋這個概念，我們需要先明確：

1、系統

指封裝了自身的資料和行為，並能獨立對外提供服務的實體。一個系統可能包含多個子系統。

2、執行者

指在研究系統外，與該系統發生功能性互動的其他人或系統。

說明：

系統執行者一定是在系統外的，可以是人或其他系統；

系統執行者必須與系統互動；

系統執行者不一定是業務執行者。

3、涉眾

主要指與要建置的業務系統相關的一切人和事，包括系統執行者在內。

以小區物業為例，當業主需要進行公區設備報修時，他們會透過微信告訴管家在小區什麼地方、哪個設備有問題需要修理，然後管家透過企微對話框提交報修單。研究該場景的報修下單系統，我們可以得到：

系統執行者：管家。儘管業主是物業服務組織的業務執行者，但直接與報銷下單系統進行互動的是管家，因此管家是該系統的系統執行者。

涉眾：包括業主、管家、工程人員和專案經理等等，因為他們都是報修下單系統的利害關係人。
![image](https://github.com/11024129/11024129..11024126/blob/main/%E5%9C%96%E4%BA%8C.jpg)
因此，從上述分析可以得出提交報修單和查看報修單滿足系統用例的概念。以下是該系統用例圖：
![image](https://github.com/11024129/11024129..11024126/blob/main/%E5%9C%962.drawio.png)
與業務用例不同，在研究系統用例時，我們需要將視角切換到系統本身，從系統的角度出發，考慮系統能夠為執行者提供什麼樣的價值，並且該價值是涉眾都可以接受的。
# 2.3 用例UML關係
系統用例圖中關係主要有四種，分別是關聯、包含、擴充、泛化，如下所示：
![image](https://github.com/11024129/11024129..11024126/blob/main/%E5%9C%96%E4%B8%89.jpg)
# 03 架構建模的工程實踐
業務需求背景：為滿足公司對小區物業電梯設備的同一線上管理需求，包括設備台帳、維修管理以及設備工單執行作業標準等業務流程，我們計劃進行電梯監管的系統建設。
![image](https://github.com/11024129/11024129..11024126/blob/main/%E5%9C%96%E5%9B%9B.jpg)
# 1、使用案例檢視：業務用例、系統用例
從需求背景中，我們可以看到電梯設備需要大量的資訊的管理，包括維護、修理、設備記錄以及統一​​的外部供應商如何管理維修電梯的標準。為了保障電梯的安全使用，我們需要將視角放在組織外部，並專注於執行者（安全監管員）的需求和價值。

透過分析，我們可以看出該系統的核心價值是確保電梯能夠安全使用，防止重大故障隱患和安全事故。基於此，我們可以進行業務用例和系統用例的梳理：
![image]()
# 2、流程視圖
我們對電梯監管的每個系統用例（業務的靜態）進行了拆解，並分析了每個場景的業務時序圖（業務的動態）。在設計系統運行時，我們重點關注了實際業務流程中的系統互動。透過直覺呈現完成系統用例的動態互動過程，我們可以清楚地看到涉及的使用者角色和外部系統。以下是電梯工單維保的業務時序圖範例：
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E5%85%AB.jpg)
# 3.邏輯視圖
邏輯視圖是面向系統邏輯分析和設計的，用於描述系統邏輯結構的視圖。在電梯監管中系統中，我們採用領域模組為核心，基於六角形分層架構想法的專案模組分析模型。其中，domain-context領域模組可以拆分核心域、支撐域和通用域模組。這些模組的劃分將在後續的工程編碼環節中反映出來：
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E4%B9%9D.jpg)
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E5%8D%81.jpg)
這裡做一下補充：我們常說的DDD（領域驅動設計）會聚焦在領域建模上，因為領域建模是領域驅動設計的核心。常見的領域建模方法包括事件風暴建模和四色建模法。

# 4、部署視圖
部署視圖主要面向系統部署，描述系統的交付、安裝和部署的過程。它解決了系統安裝部署的問題，展示了系統的交付、安裝和部署關係。在我們系統中，所有的實作都在阿里雲k8s進行Pod容器部。這裡不做展開。

# 5.編碼的環節-系統工程應用
在工程實務中，我們科研架構委員會採用了六角形架構。此架構抽象化了核心的領域層，並使用適配器模式解耦對外的層次依賴。
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E5%8D%81%E4%B8%80.jpg)
基本思想：確保核心業務邏輯的穩定，領域層應作為最純粹、最少對外依賴的層次，只包含業務知識和業務規則，不過分關心技術細節的實現（如數據持久化存儲、第三方接口調用，推播訊息等）。

根據我們以上「4+1」業務建模，可以進行子域的劃分。根據子域對於業務重要性的不同，可以分為核心域、支撐域和通用域。在適配器模組，核心域是以電梯為主體的領域。

核心域：電梯域

支撐域：工單域

通用網域：使用者網域
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E5%8D%81%E4%BA%8C.jpg)
# 5.1 領域層
分層架構以領域層(domain)為核心來分層建設，領域層包含以下內容（工程結構模組說明）：
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E5%8D%81%E4%B8%89.png)
# 5.2 核心域程式碼範例
實體和值物件的領域事件、觀察者模式，監聽電梯物件發布事件的，並執行對應的維保變更和人員計畫業務邏輯：
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E5%8D%81%E5%9B%9B.png)
部分程式碼
# 5.3 倉儲層程式碼範例
Repository適配dao實現資料持久化：
![image](https://github.com/11024134/final/blob/main/%E5%9C%96%E5%8D%81%E4%BA%94.png)
部分程式碼
# 5.4 兩個注意點
1、關注業務域的劃分

業務域劃分不清晰的系統往往更容易腐化，因為一旦業務域劃分比較亂，分不清應該放在哪個域的模組中，再經過時間和人員迭代，系統會愈發的混亂。例如，在我們應用中，有一部分介面定義放在了通用域的服務中，但從業務域劃分上來看，並不夠通用，因此將其放在通用域的服務是不合適的。我們需要單獨定義專門的業務域的套件來管理該業務專門的服務介面。

2、適度設計

在設計業務系統時，避免表達式的邏輯來實現業務。不要為追求Configuration而嘗試把所有的業務邏輯以表達式的方式進行表述。如果業務邏輯落在資料庫或Diamond裡變成片段化的QLExpress或SPEL，那麼在工程程式碼內完全看不懂系統處理了什麼業務邏輯。這樣會導致業務邏輯散一地，可讀性不高，維護成本也會增加。我們需要回到設計的初衷，設計是為了解決問題的，而不是為單純的秀設計。簡單即好，適度設計。

04 總結與展望
以業務為導向進行架構設計建模，將業務知識沉澱到設計模型中。透過系統的架構建模，可以進行整體的業務流程梳理和第三方系統的資料交互，確認系統建置的價值及目標。同時，在進行編碼前對關鍵的技術模組、分層結構做好解耦的基礎，指導後續業務代碼的程式設計。

我們在熟悉建模的流程和理論方法後，期望能夠做到一天甚至半天完成一個系統的業務流程的組織建模。接下來可以快速完成核心實作模組的分層設計，進一步完成模組的領域事件、狀態機、資料持久化等核心程式碼實作。這樣可以框定後續的業務編碼的邊界，職責單一，只剩下業務邏輯代碼的實作。
