# **Adobe Devcraft 25 - Team Torch-Bearers**  

## **Team Members**  
📌 **Sudhindra Pai, Shuddhabrota Banerjee, Shreshth Garg, Soham Ingole**  

---

## **Project Overview**  
This project focuses on **Real-Time Bidding (RTB) optimization** using a **Deep Cross Network (DCNv3)** model and other methods for efficient, interpretable, and high-performance **Click-Through Rate (CTR) and Conversion Rate (CVR) prediction**.  

### **Key Highlights**  
- **Problem Statement:** Optimize RTB bidding to maximize **clicks + N × conversions** under budget constraints.  
- **Model Used:** **DCNv3** (Deep Cross Network v3) for explicit low-order and high-order feature interactions.  
- **Comparison with Other Models:** XGBoost, LightGBM.  
- **Evaluation Metrics:** CTR, CVR, Log Loss, AUC.  

---

## **Dataset Overview**  
The dataset consists of **processed DSP bidding logs**, including bid, impression, click, and conversion data from **5 advertiser campaigns**.  

### **Files Provided**  
- **Bid Data:** `bid.06.txt` to `bid.12.txt`  
- **Click Data:** `clk.06.txt` to `clk.12.txt`  
- **Conversion Data:** `conv.06.txt` to `conv.12.txt`  
- **Impression Data:** `imp.06.txt` to `imp.12.txt`  

### **Key Features in the Dataset**  

| **Feature Name**   | **Description**                                      |
|------------------|--------------------------------------------------|
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

## **Data Preprocessing & Feature Engineering**  

### **🧹 Data Cleaning Steps**  
✔ **Dropped high-cardinality columns** (e.g., unique IDs, raw text fields).  
✔ **Filled missing values** in ad slot, region, and user data using median/mode imputation.  
✔ **Converted timestamp** to meaningful features.  

### **📊 Feature Engineering**  

#### **1️⃣ Categorical Feature Encoding**  
✔ **Multi-Label Binarization (MLB)** → Encoded **user profile tags** for better representation.  
✔ **One-Hot Encoding (OHE)** → Applied to **low-cardinality categorical features** (e.g., `Adslot Format`, `Ad Exchange`).  
✔ **Ordinal Encoding** → Used for **medium-cardinality categorical features** (e.g., `Region`, `City`, `User-Agent`).  
✔ **Frequency Encoding** → Applied to **high-cardinality categorical features** (e.g., `AdvertiserID`, `Ad Exchange`).  

#### **2️⃣ Time-Based Features**  
✔ **Extracted Hour of the Day** → Added `"hour"` from the `timestamp` to capture user behavior trends.  

#### **3️⃣ User-Agent Parsing**  
✔ **Extracted Device Type, OS, and Browser** from `User-Agent` to create new features.  

#### **4️⃣ Adslot Features**  
✔ **Computed Adslot Area** → `Width × Height` as a new feature for better ad size representation.  

#### **5️⃣ Numerical Feature Scaling**  
✔ **Min-Max Scaling & Log Transformation** → Applied to bidding price, paying price, and adslot dimensions.  

---

## **Model Selection**  

| **Model**   | **Feature Interaction** | **Interpretability** | **Computational Cost** | **Effectiveness** |
|------------|---------------------|------------------|--------------------|--------------|
| **XGBoost**  | None | High | Low | Weak |
| **DeepFM**   | Implicit (DNN) | Low | High | Strong |
| **DCNv1/v2** | Weak explicit | Medium | Medium | Good |
| **DCNv3 (Our Choice)** | **Strong Explicit (LCN+ECN)** | High | Low | **Best** |

### **Why DCNv3?**  
✔ **Explicit Feature Interaction Modeling** (LCN for low-order, ECN for high-order).  
✔ **Efficient & Lightweight** - 50% fewer parameters than deep models.  
✔ **Self-Mask Mechanism** - Reduces noise in feature interactions.  
✔ **Tri-BCE Loss** - Adaptive supervision for robust training.  

---

## **Model Architecture**  

### **🛠 DCNv3 Components**  
🔹 **Linear Cross Network (LCN):** Captures low-order feature interactions linearly.  
🔹 **Exponential Cross Network (ECN):** Captures high-order interactions exponentially.  
🔹 **Self-Mask:** Reduces redundant feature interactions → Improves efficiency.  
🔹 **Tri-BCE Loss:** Multi-loss trade-off for better supervision.  

---

## **Hyperparameter Tuning**  

### **🔧 Optimized Parameters**  
✔ **LCN Layers:** 3  
✔ **ECN Layers:** 4  
✔ **Learning Rate:** 0.0005  
✔ **Regularization:** L2 penalty = 0.01  
✔ **Dropout:** 0.2  

### **Optimization Techniques Used**  
📌 **Best Parameters Finder**  
📌 **Early Stopping** → Prevents overfitting.  

---

## **Evaluation Strategy**  

### **📈 Metrics Used**  
- **CTR (Click-Through Rate)**  
- **CVR (Conversion Rate)**  
- **Log Loss & AUC**  

---

## **Why Not Other Models?**  

### **FM / Wide & Deep / DeepFM:**  
❌ Lacks efficient feature interaction modeling → **Suboptimal CTR prediction.**  

### **DNN-based Methods:**  
❌ Computationally expensive → **Hard to scale for real-time applications.**  

### **XGBoost:**  
❌ Simple decision trees cannot learn **complex relationships between bid prices & conversions.**  

✔ **DCNv3** → **Balance of accuracy, interpretability, and efficiency.**  

---

## **📌 Conclusion & Future Work**  

### **Final Takeaways**  
📌 **Explicit feature interaction modeling** is superior to implicit deep networks.  
📌 **Self-Mask & Tri-BCE** help remove noise & improve accuracy.  
📌 **DCNv3 is efficient, accurate, and interpretable for RTB optimization.**  

### **🔮 Future Work**  
🔹 **Improve Computational Efficiency** → Optimize training pipeline for faster inference.  
🔹 **Explore Reinforcement Learning** → Apply RL techniques to improve bidding strategy.  
🔹 **Hybrid Models** → Combine DCNv3 with LightGBM for ensemble learning.  

---

🚀 **This README provides a structured and professional overview of your project!** 🎯 Let me know if you need any additional modifications. 😊  
