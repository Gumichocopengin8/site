# get-domain-late
* execution[meta header]
* function template[meta id-type]
* std::execution[meta namespace]
* cpp26[meta cpp]

```cpp
template<class Sndr, class Env>
constexpr auto get-domain-late(const Sndr& sndr, const Env& env) noexcept;
```

## 概要
[Sender](sender.md)と[Receiver](receiver.md)間[接続(connect)](connect.md)時のカスタマイゼーションポイントとして、[実行ドメイン](default_domain.md)を取得する説明専用の関数テンプレート。


## 効果
説明用の型`Domain`を下記の通り定義したとき、`return Doamin();`と等価。

- [`sender-for`](sender-for.md)`<Sndr,` [`continue_on_t`](continue_on.md.nolink)`> == true`のとき、次のラムダ式呼び出し結果の型とする。

    ```cpp
    [] {
      auto [_, sch, _] = sndr;
      return query-or-default(get_domain, sch, default_domain());
    }();
    ```
    * query-or-default[link query-or-default.md.nolink]
    * get_domain[link get_domain.md]
    * default_domain()[link default_domain.md]

- それ以外のとき、下記リストのうち最初に妥当となる式の型、かつ`void`ではない型とする。
    - [`get_domain`](get_domain.md)`(`[`get_env`](get_env.md)`(sndr))`
    - [`completion-domain`](completion-domain.md.nolink)`<void>(sndr)`
    - [`get_domain`](get_domain.md)`(env)`
    - [`get_domain`](get_domain.md)`(`[`get_scheduler`](get_scheduler.md)`(env))`
    - [`default_domain()`](default_domain.md)


## 例外
投げない


## 備考
Senderアダプタ[`continue_on`](continue_on.md.nolink)は[`schedule_from`](schedule_from.md.nolink)と連動して、実行コンテキスト遷移制御のカスタマイゼーションポイントをSchedulerに提供する。


## バージョン
### 言語
- C++26


## 関連項目
- [`execution::connect`](connect.md)


## 参照
- [P2999R3 Sender Algorithm Customization](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2023/p2999r3.html)
- [P2300R10 `std::execution`](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2024/p2300r10.html)
