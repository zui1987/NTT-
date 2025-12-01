<NTT株の予測モデルの作成>
1987年〜2024年の NTT 株価データを使用し、Pythonで時系列データを基に翌日の終値を予測する機械学習モデルを構築しています。
使用モデルは LightGBM を使用し、特徴量としてテクニカル指標（MA、RSI、MACD、BB、出来高、OBV）、および Lag 特徴量（過去1日・3日・5日の株価）を用いています。
初めに必要なライブラリをインストールしました。使用したライブラリ配下です。
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy  as np
import sklearn
from lightgbm import LGBMRegressor
from sklearn.model_selection import TimeSeriesSplit
from lightgbm import log_evaluation
from sklearn.metrics import r2_score
使用しデータでは以下の列を使用しました。
日付（date）、終値（end）、始値（start）、高値（max）、安値（min）、出来高（event）、変化率（raito）
データの前処理としてヘッダーの変更、日付の変換、出来高・変化率（%）の数値変換を行いました。
今回の学習モデルはLightGBMを使用し、8割を学習、2割をテストデータとして使用しています。
split = int(len(data) * 0.8)
X_train = X.iloc[:split]
y_train = y.iloc[:split]
X_test = X.iloc[split:]
y_test = y.iloc[split:]
model = LGBMRegressor(
    n_estimators=800,
    learning_rate=0.02,
    subsample=0.9,
    colsample_bytree=0.9
)
その後モデルを可視化し、R²で評価を行いました。
結果をもとに課題を考え改善を行いました。
