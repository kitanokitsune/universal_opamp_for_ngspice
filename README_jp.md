[English is here](README.md)
# ngspice 互換 汎用オペアンプ・モデル

ngspice 互換の汎用オペアンプ・モデルです。LTspice の UniversalOpamp (Level 3a) 相当の機能を提供します。

## 1. 等価回路
![Schematic](schematic.png)

本モデルは、以下のリソースを参考に設計されています。

* [Mastering Electronics Design - Building an Op-Amp SPICE Model](https://masteringelectronicsdesign.com/buildi-an-op-amp-spice-model-from-its-datasheet/)
* [eCircuitCenter - Opamp Models](https://www.ecircuitcenter.com/OpModels/OpampModels.htm)
* [Imperial College London - Lecture 4: Understanding Op-amp Models](http://www.ee.ic.ac.uk/pcheung/teaching/EE2_CAS/)

## 2. 特徴
* **2種類のモデル**: 1-pole モデル（`UniversalOpAmp2`）と 2-pole モデル（`UniversalOpAmp3a`）をサポートしています。
* **LTspice 互換**: LTspice の Universal Opamp モデル Level 3a と同等の機能を提供します（※ノイズ解析は除く）。
* **出力インピーダンス対応**: Open-Loop 出力インピーダンス ($R_{out}$) をサポートしています。
* **KiCAD 互換**: KiCAD の `Simulation_SPICE/OPAMP` モデルとピン互換です。

## 3. 必要条件
* [ngspice](https://ngspice.sourceforge.net/)

## 4. クイックスタート
test.cir\:
```
.title MCP6021 Open Loop Gain = -V(n_out)/V(n_in)
.include "UniversalOpAmp4NGS.lib"
.save V(n_in)
.save V(n_out)
.ac dec 100 1 100Meg

V1  n_in n_out  DC 0 SIN( 0 0 1k 0 0 0 ) AC 1
R1  n_out 0  10k
C1  n_out 0  60p
XU1 0 n_in n004 n005 n_out  UniversalOpAmp3a
+ Avol=1.1Meg GBW=11.5Meg Slew=7Meg Vos=500uV ilimit=22mA
+ railp=15mV railn=10mV Ron=20 Rout=115 Rincm=1e13 Rindiff=1e13
+ phimargin=89
V2  n004 0  DC 2.5
V3  0 n005  DC 2.5

.end
```

Exec:
```bash
ngspice -b -r test.raw -o test.log test.cir
```

## 5. パラメータ一覧

| パラメータ | 説明 | 単位 |
| :--- | :--- | :--- |
| `Avol` | オープンループ電圧利得 | [V/V] |
| `GBW` | 利得帯域幅積 | [Hz] |
| `Slew` | スルーレート | [V/sec] |
| `Vos` | 入力オフセット電圧 | [V] |
| `ilimit` | 出力短絡電流 | [A] |
| `railp` | 正電源(V+)から最大出力までのマージン | [V] |
| `railn` | 負電源(V-)から最小出力までのマージン | [V] |
| `Ron` | 出力トランジスタのオン抵抗 | [$\Omega$] |
| `Rout` | オープンループ出力インピーダンス | [$\Omega$] |
| `Rincm` | 同相入力インピーダンス | [$\Omega$] |
| `Rindiff` | 差動入力インピーダンス | [$\Omega$] |
| `phimargin` | 位相余裕（2-poleモデルのみ） | [deg] |

## 6. ライセンス
[MIT License](LICENSE)
