# User Profile Prediction

<img width="1066" height="600" alt="모델그림_가로" src="https://github.com/user-attachments/assets/a309ef87-1690-48d8-841a-b0d442b015df" />


This repository contains project that predicts user profiles from text data, using the BERT model.

By analyzing user-written text, the model predicts multiple profile attributes such as gender, age group, and more. l The results are visualized through an interactive Streamlit interface.

On the Streamlit website, you can explore prediction results through various interactive visualizations. The interface shows the distribution of predicted categories. It also allows comparison between variables. (e.g., selecting gender and age group lets you see how age groups are distributed within each gender.)

Additionally, the prediction results can be downloaded as a CSV file.

## Data Format

`bert_main.py` requires input data with **'text'** column for embedding extraction and y columns.

```
| text    | GENDER | AGE |...
| ------- | ------ | --- |
| *text*  |   0    |  0  |
| *text*  |   1    |  2  |
| *text*  |   1    |  5  |
```

`main.py` requires input data with raw text column **'text'**, extracted embedding column **'EMBEDDING'**, and y columns.

```
| TEXT    | EMBEDDING | GENDER | AGE |...
| ------- | --------- | ------ | --- |
| *text*  |  *vector* |   0    |  0  |
| *text*  |  *vector* |   1    |  2  |
| *text*  |  *vector* |   1    |  5  |

```

## Repository Structure

```python
├── bert_main.py         # KoBERT embedding extraction
├── main.py              # Model training/saving
├── streamlit_main.py    # Streamlit web application
├── data/                # Data directory
│ ├── final_data.pkl     # Data after embedding extraction
│ └── data.csv           # Pre-processed data
├── models/              # Saved models, encoders directory
├── utils/               # Utility folder
│ ├── model.py           # Model functions
│ ├── path.py            # Path configuration
│ ├── streamlit_utils.py # Streamlit utility functions
│ └── utils.py           # General utility functions
├── Dockerfile           # Docker image setup
├── compose.yml          # Docker Compose configuration
├── requirements.txt     # Python dependencies
└── README.md
```

## Getting Started

This project is intended to be run inside a Docker container with GPU support.

### Docker

```bash
docker compose up --build
```

### 1. Extract BERT Embeddings

Run `bert_main.py` to extract sentence embeddings:

```python
python bert_main.py --dataset data \
                    --type csv\
                    --model_ckpt bert-base-uncased\
                    --chunk_size 10000\
                    --batch_size 16
```

### 2. Train the Model

Run `main.py` to train the multi-output classification model:

```python
python main.py --dataset final_data \
			   --dropout 0.3 \
			   --learning_rate 0.0015 \
			   --validation_split 0.125 \
			   --batch_size 32 \
			   --epochs 100
```

### 3. Visualize the Results

Run `streamlit_main.py` to check the visualized result on web.

```python
streamlit run streamlit_main.py
```
