# molecular-color-explorer

分子の吸収スペクトル（UV-Vis励起遷移）をTDDFT（Time-Dependent DFT）計算により予測するプロジェクトです。

## 概要

SMILESで記述された分子構造から3D座標を生成し、PySCFを用いた量子化学計算（TDDFT）で励起状態と吸収スペクトルを計算します。計算結果は波長（nm）、振動子強度、分子軌道遷移情報として出力されます。

## 依存ライブラリ

| ライブラリ | バージョン | 用途 |
|-----------|-----------|------|
| PySCF | 2.11 | 量子化学計算（DFT/TDDFT） |
| GPU4PySCF | 1.4.3 (CUDA 12.x) | GPU高速化 |
| RDKit | latest | 分子構造生成・処理 |
| py3Dmol | latest | 3D構造可視化 |
| CuPy | 13.6.0 (CUDA 12.x) | GPU計算 |
| CuTensor | 1.7.0 (CUDA 12) | テンソル計算 |
| NumPy | - | 数値計算 |
| Pandas | - | データ処理・集計 |

## システム要件

- **GPU**: NVIDIA GPU（CUDA 12.x対応）
- **Python**: 3.7以上
- **OS**: Linux/macOS/Windows（GPU環境推奨）

## インストール

Google Colabまたは対応するJupyter環境で実行してください：

```bash
!pip3 install gpu4pyscf-cuda12x==1.4.3
!pip install cutensor-cu12==1.7.0
!pip install cupy-cuda12x==13.6.0
!pip install rdkit
!pip install pyscf==2.11
!pip install py3Dmol
```

## 使用方法

### ノートブック: pyscf_absorption_prediction.ipynb

PySCFを使用して分子の吸収スペクトル（励起遷移）を予測するJupyterノートブックです。

#### 処理フロー

1. **インストール** - 必要なライブラリの環境セットアップ
2. **RDKit** - SMILESから分子構造を生成し、3D座標を最適化
3. **3D構造可視化** - py3Dmolで構造の妥当性を目視確認
4. **PySCF TDDFT計算** - Time-Dependent密度汎関数理論で励起状態を計算
5. **分子軌道可視化** - HOMO・LUMO等の軌道を立体表示（オプション）
6. **データ集計** - 計算結果の要約と2次元マップ化
7. **結果確認・ダウンロード** - スペクトル画像、遷移詳細表、構造マップなどを出力

#### 主な出力ファイル

計算結果は `results/{InChIKey}/` ディレクトリに保存されます：

- `{InChIKey}_spectrum.png` - UV-Visスペクトル図
- `{InChIKey}_{calc_tag}.json` - 全計算結果（JSON形式）
- `{InChIKey}_{calc_tag}_transitions.csv` - 分子軌道遷移の詳細
- `{InChIKey}_{calc_tag}.chk` - PySCFチェックポイントファイル
- `{InChIKey}_{calc_tag}.xyz` - 最適化後の分子座標
- `orbitals_{calc_tag}/` - 軌道cubeファイル（可視化時）

#### パラメータ設定

ノートブック内で以下を指定可能：

- `CALC_BASIS` - 基底関数（"6-31G*"など）
- `CALC_XC` - 汎関数（"B3LYP"など）
- `N_STATES` - 計算する励起状態数
- `SCF_GRID_LEVEL` - グリッドレベル（計算精度）
- `CHARGE` - 分子の電荷

## 出力形式

### JSON結果

各計算の詳細は以下を含むJSON形式で保存：

- SMILES・InChIKey
- 全励起状態の遷移エネルギー・波長・振動子強度
- 許容遷移（振動子強度 > 0）のみの情報
- 分子軌道遷移係数
- 計算条件・計算時間
