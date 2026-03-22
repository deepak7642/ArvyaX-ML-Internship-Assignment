# ***🌿 ArvyaX Machine Learning Internship Assignment***

*Theme: From Understanding Humans → To Guiding Them*

At ArvyaX, we are building AI systems that go beyond prediction.

#### **We aim to create intelligence that can:**

* understand human emotional state
* reason under imperfect and noisy signals
* decide meaningful next actions
* guide users toward better mental states

#### Tasks :

* Part 1 — Emotional State Prediction
* Part 2 — Intensity Prediction
* Part 3 — Decision Engine (What + When)
* Part 4 — Uncertainty Modeling
* Part 5 — Feature Understanding
* Part 6 — Ablation Study
* Part 7 — Error Analysis
* Part 8 — Edge / Offline Thinking
* Part 9 — Robustness


---

###  **Methodology (Solution)**

* Processed user inputs combining **journal text and contextual signals** (energy level, stress level, time of day, sleep, previous mood) to capture both emotional and situational context.

* Performed **data preprocessing and missing value handling**:

  * `sleep_hours` → filled with **median values**
  * `previous_day_mood` → filled with **"unknown"**
  * `face_emotion_hint` → filled with **"missing"**

* Generated **text features using TF-IDF vectorization** and merged them with structured metadata to create a unified feature space.

* Engineered additional features to enhance model performance:

  * `sentiment`
  * `text_length`
  * `word_count`
  * `stress_energy_ratio`
  * `emotional_signal`
  * `sleep_deficit`

* Built a **dual-model architecture**:

  * Applied **Linear SVM (with calibration)** for emotional state classification.
  * Applied **Random Forest Classifier** for intensity prediction to capture non-linear relationships.

* Performed **train-test split** and evaluated models using accuracy and classification reports.

* Conducted an **ablation study** to compare model performance:

  * **Emotional State (SVM)**:

    * Text + Metadata → **77.27%**
    * Text Only → **75.76%**
  * **Intensity (Random Forest)**:

    * Text + Metadata → **28.03%**
    * Text Only → **25.00%**

  👉 Insight:

  * Metadata improves performance in both tasks
  * Significant gain in contextual understanding when combining features

* Implemented **uncertainty modeling**:

  * Generated confidence scores from models
  * Defined `uncertain_flag` to identify low-confidence predictions

* Designed a **rule-based Decision Engine**:

  * Converts predictions into:

    * `what_to_do`
    * `when_to_do`
    * `supportive_message`
  * Uses inputs: predicted state, intensity, stress, energy, and time of day

* Incorporated **context-aware logic** to deliver personalized and actionable wellness recommendations.

* Performed **feature importance analysis**:

  * Random Forest → feature importance scores
  * SVM → coefficient-based importance

* Conducted **error analysis** to identify failure cases:

  * Ambiguous text
  * Conflicting signals
  * Short inputs
  * Noisy labels

* Integrated the full pipeline into a **Gradio-based UI** for interactive demonstration and real-time decision output.

* Designed the system to support future extension into an **adaptive, personalized wellness assistant** with improved decision intelligence.

---

### 🧠 **Error Analysis**


#### 🔹 **1. Emotional State Prediction Errors**

* Total misclassifications observed: **30 cases**

#### 📌 Key Failure Patterns:

* **Ambiguous / Short Text**

  * Examples: *“felt heavy”*, *“okay session …”*
  * Issue: Lack of sufficient context leads to incorrect classification
  * Model struggles to infer clear emotional state from minimal input

---

* **Mixed / Transitioning Emotions**

  * Example:
    *“felt good for a moment. Later it changed…”*
  * Issue: Presence of multiple emotions in a single text
  * Model predicts only one dominant class → misses emotional shifts

---

* **Conflicting Signals**

  * Example:
    *“back to normal after… then shifted again”*
  * Issue: Contradictory emotional cues within the same sentence
  * Leads to unstable predictions

---

* **Implicit Emotions**

  * Example:
    *“got distracted again”*
  * Issue: Emotion not explicitly stated
  * Requires deeper semantic understanding beyond keywords

---

#### 📌 Root Causes:

* TF-IDF captures **surface-level word importance**, not deep meaning
* Lack of sequence/context understanding
* No handling of **emotion transitions over time**

---

---

#### 🔹 **2. Intensity Prediction Errors**

* Total misclassifications observed: **95 cases** (significantly higher than emotional state)

---

#### 📌 Key Failure Patterns:

* **Subtle Intensity Differences**

  * Example:
    *“felt lighter than before”* (true: 4, predicted: 5)
  * Issue: Model struggles to distinguish **close intensity levels**

---

* **Mixed Intensity Signals**

  * Example:
    *“part of me relaxed while another part…”*
  * Issue: Multiple intensity cues → model averages incorrectly

---

* **Overestimation / Underestimation**

  * Example:
    *“that helped a little”* → predicted lower than actual
  * Issue: Weak textual signals lead to biased predictions

---

* **Dependence on Text Over Context**

  * Example:
    *“felt distracted…”* but actual intensity high
  * Issue: Model does not fully leverage metadata (stress, energy)

---

#### 📌 Root Causes:

* Intensity is inherently **subjective and harder to quantify**
* Random Forest struggles with:

  * Fine-grained ordinal prediction (1–5 scale)
* Limited signal strength in short text inputs

---

---

#### 🔹 **3. Key Insights from Error Analysis**

* Emotional state prediction performs **better than intensity prediction**
* **Text-only signals are insufficient** for accurate intensity estimation
* **Metadata plays a critical role**, but needs stronger integration
* Errors increase significantly when:

  * Text is short
  * Emotions are mixed
  * Signals are conflicting

---

---

#### 🔹 **4. How to Improve**

#### 🔧 Data Improvements

* Increase dataset size with **richer and longer text samples**
* Improve **label consistency** for intensity
* Add more features (like):

  * Habit patterns
  * Behavioral addiction
  * Lifestyle risks
  
---

#### 🔧 Modeling Improvements

* Replace TF-IDF with **contextual embeddings (BERT / Transformer models)**
* Use **ordinal regression models** for intensity prediction
* Try **ensemble models** combining multiple classifiers

---

#### **Conclusion from Error Analysis**

* The system performs well for **clear and strong emotional signals**
* Performance drops in:

  * ambiguous
  * mixed
  * low-context scenarios
* Improving **context understanding + feature richness** is key to enhancing performance

---


### Part 8 — Edge / Offline Thinking Plan

```text
                    📱 Mobile Device (On-Device Execution)
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│   👤 User Input Layer                                        │
│   ─────────────────────                                      │
│   • Journal Text                                             │
│   • Energy Level                                             │
│   • Stress Level                                             │
│   • Time of Day                                              │
│                                                              │
│                │                                             │
│                ▼                                             │
│   🧹 Preprocessing Layer                                     │
│   ─────────────────────                                      │
│   • Missing Value Handling                                   │
│   • Text Cleaning                                            │
│                                                              │
│                │                                             │
│                ▼                                             │
│   ⚙️ Feature Engineering Layer                               │
│   ───────────────────────────                                │
│   • NLP Features (Text Vectorization)                        │
│   • Engineered Features (ratios, sentiment, etc.)            │
│   • Metadata Features                                        │
│                                                              │
│                │                                             │
│                ▼                                             │
│   🤖 ML/NLP Prediction Layer                                 │
│   ─────────────────────────                                  │
│   • Emotional State Prediction                               │
│   • Intensity Prediction                                     │
│   • Confidence Score Generation                              │
│                                                              │
│                │                                             │
│                ▼                                             │
│   ⚠️ Uncertainty Module                                      │
│   ─────────────────────                                      │
│   • Confidence Thresholding                                  │
│   • Uncertain Flag Detection                                 │
│                                                              │
│                │                                             │
│                ▼                                             │
│   🧠 Decision Engine (What + When)                           │
│   ─────────────────────                                      │
│   • What to Do                                               │
│   • When to Do                                               │
│   • Context-Aware Logic                                      │
│                                                              │
│                │                                             │
│                ▼                                             │
│   💬 Output Layer                                            │
│   ─────────────────────                                      │
│   • Action Recommendation                                    │
│   • Timing Suggestion                                        │
│   • Supportive Message                                       │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

### **Part 9 — Robustness**

#### 🔹 **Handling Very Short Text (e.g., “ok”, “fine”)**

* Short inputs provide **limited semantic information**, making prediction difficult.
* The system handles this by:

  * Relying more on **metadata features** (stress, energy, time of day)
  * Using **engineered features** such as `text_length` and `word_count` to detect low-information inputs
  * Leveraging **confidence scores**:

    * Low confidence → `uncertain_flag = 1`
* In such cases, the Decision Engine:

  * Avoids strong recommendations
  * Provides **safe, generic actions** (e.g., pause, light breathing)

---

#### 🔹 **Handling Missing Values**

* Missing data is handled during preprocessing to ensure stability:

  * `sleep_hours` → filled with **median values**
  * `previous_day_mood` → filled with **"unknown"**
  * `face_emotion_hint` → filled with **"missing"**

* Benefits:

  * Prevents model failure due to null values
  * Maintains consistency across training and inference
  * Allows the model to treat missingness as a **valid signal**

---

#### 🔹 **Handling Contradictory Inputs**

* Contradictions may occur when:

  * Text suggests negative emotion but stress is low
  * High energy but low mood
  * Mixed emotional expressions in text

* The system addresses this using:

  * **Combined feature learning** (text + metadata)
  * **Engineered features** (e.g., stress-energy ratio, emotional signal)
  * **Uncertainty modeling**:

    * Conflicting signals → lower confidence → `uncertain_flag = 1`

* Decision Engine behavior:

  * Avoids extreme actions
  * Recommends **balanced interventions** (e.g., light activity, grounding)
  * Prioritizes **safe and non-invasive suggestions**

---

