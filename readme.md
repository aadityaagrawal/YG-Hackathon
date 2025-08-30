# 🖋️ YG Hackathon – Online Signature Verification <img src="image.png" alt="Alt Text" style="height:1.5em; vertical-align:middle;">

This project implements an **Online Signature Verification System** using a **Siamese LSTM Network**.  

Unlike image-based verification, this approach leverages **dynamic signature features** — coordinates, pressure, and timing — to detect **genuine vs. forged signatures**.

✅ Works on synthetic and realistic mock data  
✅ Detects skilled forgeries based on signature dynamics  
✅ Provides visualization for easy comparison  

---

## Workflow Diagram

<!-- ![Workflow Diagram](./model.png) -->
*Figure: Signature verification pipeline*

```
                +-----------------------------------+
                |      Signature Verification       |
                +-----------------+ +---------------+
                |  Signature A    | |  Signature B  |
                | (x, y, pressure,| | (x, y, pressure,|
                | dt)             | | dt)           |
                +-----------------+ +---------------+
                             |           |
                             v           v
                +-----------------------------------+
                |          Pre-processing           |
                |     (Normalize & Pad to MAX_LEN)  |
                +-----------------------------------+
                             |           |
                             v           v
                +-----------------------------------+
                |     Siamese LSTM Network          |
                |     (Shared Weights Model)        |
                +-----------------------------------+
                             |
                             v
                +-----------------------------------+
                |        Feature Comparison         |
                |  (Compute Euclidean Distance)     |
                +-----------------------------------+
                             |
                             v
                +-----------------------------------+
                |      Threshold Comparison         |
                |  Distance < Threshold  -> Genuine |
                |  Distance >= Threshold -> Forged  |
                +-----------------------------------+
                             |
                             v
                +-----------------------------------+
                |      Signature Pair Outcome       |
                |   (Genuine or Forged)             |
                +-----------------------------------+
```
---

## Key Features
* Dynamic Signature Features: Uses [x, y, pressure, dt] instead of static images.
* Siamese Network with Bidirectional LSTM: Learns signature embeddings and measures similarity.
* Contrastive Loss Function: Penalizes distance for genuine signatures and rewards difference for forgeries.
* Synthetic & Realistic Data Generator: Simulates genuine variations and skilled forgeries.
* Visualization: Compare two signatures side by side with plotted coordinates.
---


## Project Flow

1. Generate Signature Data

    1. Synthetic signatures with random noise and dynamics.
    2. Realistic mock signatures simulating human handwriting variations.

2. Preprocess Signatures

    1. Normalize coordinates to origin.
    2. Scale to 1x1 bounding box.
    3. Pad sequences to same length.

3. Siamese LSTM Model

    1. Bidirectional LSTM layers extract embeddings.
    2. Embeddings from two signatures are compared using Euclidean distance.

4. Contrastive Loss Training

    1. Minimizes distance for genuine pairs.
    2. Maximizes distance for forged pairs.

5. Prediction & Visualization

    1. Input two signatures → Model predicts distance.
    2. Distance < Threshold → Genuine, else Forged.
    3. Plot both signatures for visual confirmation.
---

## Sample Testing
* Genuine Pair → Distance = 0.12 → Predicted as Genuine ✅
* Forged Pair → Distance = 0.43 → Predicted as Forged ❌

Visualization of signatures is automatically plotted for manual inspection.
---

## Usage
```
from signature_verification import predict_similarity

predict_similarity(
    signature1, signature2, 
    model=siamese_model,
    max_len=MAX_LEN, 
    threshold=THRESHOLD
)
```

* signature1, signature2: Arrays of [x, y, pressure, dt]
* model: Trained Siamese model
* max_len: Maximum signature length for padding
* threshold: Distance threshold for Genuine/Forged classification
---

## Technologies
* Python 3.x
* TensorFlow 2.x / Keras
* NumPy
* Matplotlib
---

## Future Improvements
1. Train on real datasets like SVC2004 or MCYT
2. Learn optimal threshold dynamically
3. Add velocity & angular features to improve accuracy
4. Deploy as a web application for real-time verification
---

## Why This Approach?
1. Dynamic features (pressure & speed) make the system resistant to skilled forgery.
2. Siamese architecture allows learning a similarity metric rather than classifying each signature independently.
3. Visualization provides intuitive feedback for users and auditors.
