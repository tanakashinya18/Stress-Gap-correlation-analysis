# データ分析：ストレスと印象・味覚ギャップの相関解析

## 概要
本プロジェクトでは、異なる色環境下でのストレス指標（LF/HF比）と印象・味覚ギャップの関係を解析します。Python を用いた相関分析と回帰分析を行い、データを可視化します。

## 目的
- LF/HF 比を指標としてストレスレベルを算出する。
- 印象評価と味覚評価のギャップを計算し、それとストレス値の相関を解析する。
- 色環境ごとの影響を分析し、統計的な関係を明確化する。

## 使用技術
- Python 3
- NumPy, Pandas
- SciPy
- Matplotlib
- Excel データ処理

## 使用方法
### 1. 必要なライブラリのインストール
以下のコマンドで必要な Python ライブラリをインストールしてください。
```sh
pip install numpy pandas scipy matplotlib
```

### 2. スクリプトの実行
```sh
python stress_gap_analysis.py
```

### 3. 出力データ
- `相関係数_苦味_ストレスとギャップ.csv`：色環境ごとのストレス値と印象・味覚ギャップの相関係数を記録した CSV ファイル。
- `{color}_苦味_ストレスとギャップ.png`：各色環境ごとの相関グラフ。

## データフォーマット
入力データは Excel ファイルとして用意し、以下のような構造を持ちます。

### **LFHF解析（相関関係）.xlsx**（ストレスデータ）
| 被験者 | 赤 | 青 | 緑 | ピンク | 黄 | 橙 |
|--------|----|----|----|------|----|----|
| A      | 50 | 30 | 20 | 40   | 60 | 25 |
| B      | 45 | 35 | 22 | 38   | 55 | 27 |

### **印象味覚ギャップ.xlsx**（ギャップデータ）
| 被験者 | 赤 | 青 | 緑 | ピンク | 黄 | 橙 |
|--------|----|----|----|------|----|----|
| A      | 1  | 2  | 3  | 2    | 4  | 1  |
| B      | 2  | 3  | 1  | 3    | 5  | 2  |

## Python コードの説明

```python
import pandas as pd
from scipy.stats import linregress
import matplotlib.pyplot as plt

# エクセルファイルの指定
stress_file = "LFHF解析（相関関係）.xlsx"
impression_file = "印象味覚ギャップ.xlsx"
stress_sheet = "苦味"
impression_sheet = "苦味"

# データの読み込み
stress_df = pd.read_excel(stress_file, sheet_name=stress_sheet, index_col=0, usecols="A:M", nrows=7)
impression_df = pd.read_excel(impression_file, sheet_name=impression_sheet, index_col=0, usecols="A:M", nrows=7)

# データの転置
stress_df = stress_df.T
impression_df = impression_df.T

# 各色ごとの相関分析
for color in stress_df.columns:
    stress_values = stress_df[color].dropna()
    impression_values = impression_df[color].dropna()
    if len(stress_values) < 2 or len(impression_values) < 2:
        continue
    
    slope, intercept, r_value, _, _ = linregress(impression_values, stress_values)
    plt.scatter(impression_values, stress_values, alpha=0.7)
    plt.plot([0, 6], [slope * 0 + intercept, slope * 6 + intercept], color="magenta", linestyle="--")
    plt.xlabel('Gap')
    plt.ylabel('Stress')
    plt.title(f"{color} (r={round(r_value, 2)})")
    plt.savefig(f"{color}_苦味_ストレスとギャップ.png", dpi=300)
    plt.show()
```

## ライセンス
本プロジェクトは MIT ライセンスのもとで提供されます。

## 著者
近畿大学 理工学部 情報学科 システムデザイン論研究室　田中真矢

