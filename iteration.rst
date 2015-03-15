イテレート
**********

.. contents::
    :depth: 2

独自のイテレータを作りたい
==========================

BoostにはIteratorという、お手軽イテレータ作成ライブラリがあるが、
実のところ、それほどお手軽という印象はない。
特に複雑なループをしながら要素を取り出す必要がある場合に、これは顕著となる。
代わりにCoroutineというライブラリを使ったほうが直感的に記述できて楽。

.. code-block:: cpp
    :linenos:

    #include <iostream>

    #include <boost/coroutine/all.hpp>

    typedef boost::coroutines::coroutine<int>::pull_type PullType;
    typedef boost::coroutines::coroutine<int>::push_type PushType;

    // フィボナッチ数列を第n項まで列挙する
    class Fibonacci {
       public:
        PullType Enumerate(int n) {
            return PullType([n](PushType& yield) {
                auto a1 = 1;
                auto a2 = 1;

                yield(a1);
                yield(a2);

                for (int i = 2; i < n; ++i) {
                    auto a3 = a1 + a2;

                    yield(a3);

                    a1 = a2;
                    a2 = a3;
                }
            });
        }
    };

    int main() {
        Fibonacci fib;

        for (auto i : fib.Enumerate(10)) {
            std::cout << i << std::endl;
        }

        return 0;
    }
