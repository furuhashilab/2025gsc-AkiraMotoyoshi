# 2025gsc-AkiraMotoyoshi

# 実測歩行データに基づく通学ルート最適化 
（淵野辺駅北口 → 青山学院大学相模原キャンパス正門）

---

## スライド

発表スライドは[スライドはこちら](https://docs.google.com/presentation/d/1bK46tgva-Cu4hoT6Y-DhLC7425q-rJpIZl_bWpgDZA0/edit?slide=id.g3bd429196ec_2_8#slide=id.g3bd429196ec_2_8)


---

## License

This project is licensed under the  
**Creative Commons Attribution 4.0 International (CC BY 4.0)** License.

© FuruhashiLabratory/AkiraMotoyoshi  
https://creativecommons.org/licenses/by/4.0/

---

# I. Introduction（研究背景・目的）

大学生にとって通学路は日常的に利用する重要な生活動線であり、距離の短さだけでなく、歩きやすさや夜間の安全性もルート選択に影響を与える。特に徒歩通学では、信号待ちや横断、街灯の有無といった要素が実際の所要時間や心理的負担を左右する。

本研究は、**淵野辺駅北口から青山学院大学相模原キャンパスまでの登下校ルート**を対象に、  
実測データに基づき、実質的に最も効率的な通学ルートを明らかにすることを目的とする。

---![section_map](https://github.com/user-attachments/assets/538531a8-b045-4375-b56d-f42b3526e090)


# II. Methods（研究方法）
## 使用ツール / Tools

- **[QGIS 3.44](https://qgis.org/)**（2025–2026）  
  距離計測、街灯密度分析、ルート可視化、地図作成

- **[OpenStreetMap (OSM)](https://www.openstreetmap.org/)**  
  道路・信号・横断歩道・街灯データ

- **[Google Earth](https://www.google.com/earth/)**  
  景観確認、歩道状況の事前チェック

- **[Strava（無料版）](https://www.strava.com/)**（2025–2026）  
  歩行ログ（GPX）取得、移動速度・停止時間の分析

- **[GPX Track Splitter](https://mapconcierge.github.io/GPXtrackSplitter/)**（2026）  
  GPX データのセクション分割

- **[Microsoft Excel](https://www.microsoft.com/microsoft-365/excel)**（2025–2026）  
  セクション別歩行時間・距離の整理、平均値算出

- **[ChatGPT 5.2](https://chat.openai.com/)**（2025–2026）  
  研究設計、分析手法整理、数式整理、README・コード作成補助

## 1. 歩行ルートの選定と絞り込み

すべての道路を実際に歩行して計測することは、データ量・時間・組み合わせ数の観点から非現実的である。  
そのため、以下の基準に基づいて分析対象ルートを選定した。

- 明らかに遠回りとなるルートは除外  
- 距離や構造がほぼ同一のルートは代表的なものに集約  
- 実際に学生が利用していると考えられるルートを優先  

道をセクションにわけ、QGIS 上に LineString として作成し、分析対象とした。

<img src="https://github.com/user-attachments/assets/538531a8-b045-4375-b56d-f42b3526e090"width="300">

---

## 2. ルートのセクション分割

ルート全体を一括で比較すると組み合わせ数が増大し、分析が困難となる。  
そこで本研究では、ルートを**セクション単位**に分割し、後からセクションを組み合わせて評価する手法を採用した。

セクションの切れ目（ノード）は以下を基準とした。

- 主要な交差点  
- 信号のある地点  

セクションが短くなりすぎると GPS 誤差や計測誤差の影響が相対的に大きくなるため、全ての交差点でセクションを分けず、セクション数は必要最小限とした。

![section_map](https://github.com/user-attachments/assets/caa2f61c-f49e-45ff-9563-d99785b9425a)



<img width="1440" height="900" alt="routes_comparison" src="https://github.com/user-attachments/assets/3342a77b-d9c3-430b-ac14-8e5034c905e4"/>


![section_map_zoom2](https://github.com/user-attachments/assets/b609701b-b5db-402e-bcf4-d5aff3d19809)



---

## 3. Strava を用いた実地歩行計測

Strava（無料版）の Walk 機能を用い、淵野辺駅改札から大学正門までを実際に歩行し、GPX データを取得した。  
同一セクションについては**最低5回以上**歩行し、時間帯や体調による偏りを抑制した。

---

## 4. GPX データの分割と整理

取得した GPX データは **GPX Track Splitter** を用いてセクション単位に分割した。  

https://mapconcierge.github.io/GPXtrackSplitter/

![GPX_Track_Splitter.png](images/gpx_track_splitter.png)

---

## 5. セクション別平均値の算出

すべてのセクションについて、歩行時間および距離を Excel に整理し、平均値を算出した。  
平均値を用いた理由は、複数回の計測結果から偶発的な誤差を低減するためである。

[セクションタイムCSV](images/route_times_all.csv)  

![時間と距離](images/time_distance.png)  

![セクション毎 最速タイム](images/comparison1.png)

---

## 6. 信号待ち時間の算定

本研究で用いた信号待ち時間の評価方法は、**Highway Capacity Manual 2000（HCM 2000）**における歩行者平均遅延（average pedestrian delay）の考え方と整合的である。

HCMでは、信号サイクル長および有効青時間に基づき、歩行者遅延を理論的な期待値として定式化している。一方で、本研究では信号制御の詳細な運用情報が公開されていないこと、および実際の利用者視点での移動時間評価を重視する立場から、理論式の直接適用は行わず、実測データを複数回取得した上で平均値を算出する方法を採用した。

信号待ち時間は、以下の期待値モデルを用いた。

\[
E(W) = \frac{T_{\text{red}}}{2}
\]

- Point B：淵野辺駅入口交差点 → 58 秒  
- Point C：青山学院大学入口交差点 → 26.6 秒  

---

## 7. 信号のない横断歩道の処理

Stravaのデータより、信号がない横断歩道のあるヘルクレス通りを渡るために待った時間の平均は約10秒。  
横断する時間も含め、ヘルクレス通りをわたる場合には +15秒 とする。

また、他にも信号のない横断歩道（例：淵野辺105号付近）が存在するが、観測期間中において待ち時間が一切発生しなかった。  
この横断地点は車両交通量が非常に少ないことから、当該箇所については遅延時間の算入は行わないこととする。

---

# III. Results（結果）

セクション別平均歩行時間を基にルートを合成した結果、  
**最短距離ルートと最短所要時間ルートは必ずしも一致しない**ことが明らかになった。

信号待ち時間や横断回数を考慮すると、距離がやや長いルートであっても、  
停止回数の少ないルートの方が総所要時間が短くなるケースが確認された。。

![導き出された最短ルート](images/fastest_route.png)

---

# IV. Discussion（考察）

本研究より、通学ルートの評価において距離のみを基準とすることには限界があることが示された。  
信号待ちや横断による遅延は、日常的な徒歩移動において無視できない影響を及ぼす。

地図アプリで表示された数値上の最短ルートも、実測データに基づく最速ルートと一致しない場合がある。  
本研究の手法は、実際の利用者視点に立脚した、実践的な通学ルート評価手法として有効である。

---

## 🛠 使用ツール / Tools

- **QGIS**：距離計測、街灯密度分析、地図作成  
- **OpenStreetMap (OSM)**：道路・信号・横断歩道・街灯データ  
- **Google Earth**：景観確認、歩道状況のチェック  
- **Strava（無料版）**：歩行ログ（GPX）取得、移動速度・停止時間の分析  
- **ChatGPT**：研究設計、手順作成、分析補助、コーディング補助  
  
