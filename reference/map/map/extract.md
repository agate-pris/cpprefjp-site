# extract
* map[meta header]
* function[meta id-type]
* std[meta namespace]
* map[meta class]
* cpp17[meta cpp]

```cpp
node_type extract(const_iterator position); (1)
node_type extract(const key_type& x);       (2)
```

## 概要
(1) `position`が指すノードを切り離し、その要素を所有する[ノードハンドル](/reference/node_handle/node_handle.md)を返す。  
(2) `x`と等価なキーが見つかった場合、`x`が指すノードを切り離し、その要素を所有するノードハンドルを返す。それ以外の場合は空のノードハンドルを返す。 


## 戻り値
要素を所有するか、空のノードハンドル


## 計算量
(1) 償却定数時間  
(2) `map a;`のとき、log(a.size())


## 備考
`extract`は、要素に対するコピーもムーブも行わずに、要素の所有権を転送することができる。
また、`extract`は再確保なしでマップ要素のキーを変更することができる。 


## 例
```cpp example
#include <iostream>
#include <map>

int main()
{
  std::map<int, char> m1;
  std::map<int, char> m2 = {
    {10, 'a'},
    {20, 'b'},
    {30, 'c'}
  };

  // ノードを取得
  std::map<int, char>::node_type node = m2.extract(10);

  // キーを書き換え
  node.key() = 15;

  // ノードを転送
  m1.insert(std::move(node));

  std::cout << "m1 :" << std::endl;

  for (const auto& [key, value] : m1)
    std::cout << "[" << key << ", " << value << "]" << std::endl;

  std::cout << "\n" << "m2 :" << std::endl;

  for (const auto& [key, value] : m2)
    std::cout << "[" << key << ", " << value << "]" << std::endl;
}
```
* extract[color ff0000]
* node_type[link /reference/node_handle/node_handle.md.nolink]
* key[link /reference/node_handle/node_handle/key.md.nolink]


### 出力
```
m1 :
[15, a]

m2 :
[20, b]
[30, c]
```

## バージョン
### 言語
- C++17


### 処理系
- [Clang](/implementation.md#clang): 7.0.0
- [GCC](/implementation.md#gcc): 7.1.0
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): 2017 Update 5


## 関連項目
- [node_handle](/reference/node_handle/node_handle.md)


## 参照
- [Splicing Maps and Sets(Revision 5)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0083r3.pdf)

