# Adobe Devcraft 25 - Team Torch-Bearers

##  Project Overview  
This project focuses on **Real-Time Bidding (RTB) optimization** using a **Deep Cross Network (DCNv3)** model and other methods for efficient, interpretable, and high-performance **Click-Through Rate (CTR) and Conversion Rate (CVR) prediction**.

The goal is to **maximize clicks and conversions while maintaining budget constraints** in a second-price auction system used in digital advertising.

###  Key Highlights  
- **Problem Statement:** Optimize RTB bidding to maximize **clicks + N × conversions** under budget constraints.  
- **Model Used:** **DCNv3** (Deep Cross Network v3) for explicit low-order and high-order feature interactions.  
- **Comparison with Other Models:** XGBoost, LightGBM.  
- **Evaluation Metrics:** CTR, CVR, Log Loss, AUC.  

---

##  Dataset Overview  
The dataset consists of **processed DSP bidding logs**, including bid, impression, click, and conversion data from **5 advertiser campaigns**.

###  Files Provided  
- **Bid Data:** `bid.06.txt` to `bid.12.txt`  
- **Click Data:** `clk.06.txt` to `clk.12.txt`  
- **Conversion Data:** `conv.06.txt` to `conv.12.txt`  
- **Impression Data:** `imp.06.txt` to `imp.12.txt`  

###  Key Features in the Dataset  

| Feature Name    | Description                                        |
|----------------|----------------------------------------------------|
| **BidID**      | Unique identifier for each bid request.           |
| **Timestamp**  | Time of bid request (`yyyyMMddHHmmssSSS`).        |
| **Logtype**    | Impression (1), Click (2), or Conversion (3).     |
| **User-Agent** | Device, OS, and browser information.              |
| **IP, Region, City** | User location attributes.                    |
| **Ad Exchange** | Identifier for the ad marketplace.               |
| **AdslotID**   | Unique ID for ad slot.                            |
| **Adslot Width/Height** | Ad size in pixels.                        |
| **Bidding Price** | Price bid by DSP.                               |
| **Paying Price** | Second-highest bid (market price).              |
| **AdvertiserID** | Unique identifier for the advertiser.           |

---

##  Data Preprocessing & Feature Engineering  

###  Data Cleaning Steps  
- **Normalization:** Log scaling for bidding prices.  
- **Missing Value Handling:** Impute missing ad slot and user features.  

###  Feature Encoding  
- **Categorical Variables:** One-hot encoding for Adslot format, visibility.  
- **Ordinal Encoding:** Region, City, User-Agent.  
- **Frequency Encoding:** AdvertiserID, Ad Exchange.  
- **Embedding-based Features:** Used for DCNv3.  

---

##  Model Selection  

| Model   | Feature Interaction | Interpretability | Computational Cost | Effectiveness |
|---------|---------------------|------------------|--------------------|--------------|
| **XGBoost**  | None | High | Low | Weak |
| **DeepFM**   | Implicit (DNN) | Low | High | Strong |
| **DCNv1/v2** | Weak explicit | Medium | Medium | Good |
| **DCNv3 (Our Choice)** | **Strong Explicit (LCN+ECN)** | High | Low | **Best** |

###  Why DCNv3?  
✔ **Explicit Feature Interaction Modeling** (LCN for low-order, ECN for high-order).  
✔ **Efficient & Lightweight** - 50% fewer parameters than deep models.  
✔ **Self-Mask Mechanism** - Reduces noise in feature interactions.  
✔ **Tri-BCE Loss** - Adaptive supervision for robust training.  

---

##  Model Architecture  

###  **DCNv3 Components**  
- **Linear Cross Network (LCN):** Captures low-order feature interactions linearly.  
- **Exponential Cross Network (ECN):** Captures high-order interactions exponentially.  
- **Self-Mask:** Reduces redundant feature interactions → Improves efficiency.  
- **Tri-BCE Loss:** Multi-loss trade-off for better supervision.  

---

##  Hyperparameter Tuning  

### 🔧 Optimized Parameters  
 **LCN Layers:** 3  
 **ECN Layers:** 4  
 **Learning Rate:** 0.0005  
 **Regularization:** L2 penalty = 0.01 
 **Dropout:** 0.2  

###  Optimization Techniques Used  
📌 **Best Parameters Finder**  
📌 **Early Stopping** → Prevents overfitting.  

---

##  Evaluation Strategy  

###  Metrics Used  
- **CTR (Click-Through Rate)**  
- **CVR (Conversion Rate)**  
- **Log Loss & AUC**  

---

## Why Not Other Models?  

 **FM / Wide & Deep / DeepFM:**  
Lacks efficient feature interaction modeling → **Suboptimal CTR prediction.**  

 **DNN-based Methods:**  
Computationally expensive → **Hard to scale for real-time applications.**  

 **XGBoost:**  
Simple decision trees cannot learn **complex relationships between bid prices & conversions.**  

✔ **DCNv3** → **Balance of accuracy, interpretability, and efficiency.**  

---

## 📌 Conclusion & Future Work  

### Final Takeaways  
📌 **Explicit feature interaction modeling** is superior to implicit deep networks.  
📌 **Self-Mask & Tri-BCE** help remove noise & improve accuracy.  
Future Work, will involve refinement of pipeline, improving upon the computational efficiency of the model, without compromising on the accuracy. Explore RF strategies on top of the DCN architecture

---

