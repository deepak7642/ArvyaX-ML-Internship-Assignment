# ***🌿 ArvyaX Machine Learning Internship Assignment***

*Theme: From Understanding Humans → To Guiding Them*

At ArvyaX, we are building AI systems that go beyond prediction.

**We aim to create intelligence that can:**

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

