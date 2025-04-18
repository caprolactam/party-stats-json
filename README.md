総務省の公開する国政選挙における比例代表の市町村別得票数（たとえば[第50回衆議院議員総選挙における市区町村別得票数](https://www.soumu.go.jp/senkyo/senkyo_s/data/shugiin50/index.html)）をJSONに加工したものです。具体的な出典については各JSONの`source`プロパティを参考にしてください。

## データの利用

独自の利用条件はありません。ただし、[総務省の利用規約](https://www.soumu.go.jp/menu_kyotsuu/policy/tyosaku.html#tyosakuken)に従ってください。

## データの取り扱い

以下の理由により、出典元のデータとは一致しない場合があります。

### 小数点以下の取り扱いによる違い

元のデータから小数点以下三桁目まで取得し、四桁目以降を切り捨てています。

### 行政区分と選挙の区分けが異なる場合

選挙区が二つ以上の市区町村にまたがった状態で集計されている場合、独断によりどちらか一方の市区町村に割り当てています。

[第48回](https://www.soumu.go.jp/senkyo/senkyo_s/data/shugiin48/shikuchouson_15.html)および[第49回衆議院議員総選挙の新潟県](https://www.soumu.go.jp/senkyo/senkyo_s/data/shugiin49/shikuchouson_15.html)において新潟市江南区、新潟市北区はまとめて集計されており、新潟市江南区に割り当てています。

[第48回](https://www.soumu.go.jp/senkyo/senkyo_s/data/shugiin48/shikuchouson_14.html)および[第49回衆議院議員総選挙の神奈川県](https://www.soumu.go.jp/senkyo/senkyo_s/data/shugiin49/shikuchouson_14.html)において川崎市高津区と中原区、川崎市多摩区と宮前区はそれぞれまとめて集計されているところ、それぞれ川崎市高津区、川崎市多摩区に割り当てています。

### 元データに不一致があり正しいデータが確定できない場合

たとえば、[第25回参議院議員総選挙](https://www.soumu.go.jp/senkyo/senkyo_s/data/sangiin25/index.html)において長野県全体の「安楽死制度を考える会」の得票総数が、都道府県別党派別得票数（比例代表）および長野県候補者別市区町村別得票数で異なります（「労働の解放をめざす労働者党」についても同様）。このため長野県ないし全国での得票数を確定させることができません。

このリポジトリでは、各市町村別得票数のデータが正しいものとみなして集計を行っています。

## データの型

`all/*.json`

```ts
type All = {
  name: string; // 選挙名
  source: string; // 出典
  electionType: 'shugiin' | 'sangiin';
  date: string; // YYYY-MM-DD
  result: {
    votes: Array<{
      party: string; // 政党名
      count: number; // 得票数
    }>;
    totalCount: number; // すべての政党の得票数の合計
  }
}
```

`regions/*.json`

```ts
type Region = {
  name: string; // 選挙名
  source: string; // 出典
  electionType: 'shugiin' | 'sangiin';
  date: string; // YYYY-MM-DD
  result: Array<{
    name: string; // 地方名
    votes: Array<{
      party: string; // 政党名
      count: number; // 得票数
    }>;
    totalCount: number; // 各地方におけるすべての政党の得票数の合計
  }>
}
```

`prefectures/*.json`

```ts
type Prefectures = {
  name: string; // 選挙名
  source: string; // 出典
  electionType: 'shugiin' | 'sangiin';
  date: string; // YYYY-MM-DD
  result: Array<{
    name: string; // 都道府県名
    code: string; // 全国地方公共団体コード cf. https://www.soumu.go.jp/denshijiti/code.html
    votes: Array<{
      party: string; // 政党名
      count: number; // 得票数
    }>;
    totalCount: number; // 各都道府県におけるすべての政党の得票数の合計
  }>
}
```

`cities/*.json`

```ts
type Cities = {
  name: string; // 選挙名
  source: string; // 出典
  electionType: 'shugiin' | 'sangiin';
  date: string; // YYYY-MM-DD
  result: Array<{
    name: string; // 市区町村名
    /**
     * 行政区域コード(N03_007) cf. https://nlftp.mlit.go.jp/ksj/gml/codelist/AdminAreaCd.html
     * ≠ 全国地方公共団体コード
    **/
    code: string;
    votes: Array<{
      party: string; // 政党名
      count: number; // 得票数
    }>;
    totalCount: number; // 各市区町村におけるすべての政党の得票数の合計
  }>
}
```
