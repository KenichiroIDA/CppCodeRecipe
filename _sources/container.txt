コンテナ
********

.. contents::
    :depth: 2

std::vectorに複数要素を楽に追加したい
=====================================

C++11
-----

.. code-block:: cpp
    :linenos:

    #include <vector>

    int main() {
        std::vector<int> v = {0, 1, 2, 3, 4};
    }

C++11以前
---------

コンテナをストリーム化する方法がある。

.. code-block:: cpp
    :linenos:

    #include <vector>

    template <typename Container>
    class cstream {
       public:
        typedef typename Container::value_type value_type;

        cstream() : cont_(NULL) {}
        explicit cstream(Container& cont) : cont_(&cont) {}
        ~cstream() {}

        cstream<Container>& operator<<(const value_type& v) {
            cont_->push_back(v);
            return *this;
        }

       private:
        Container* cont_;
    };

    int main() {
        std::vector<int> v;
        cstream<std::vector<int> > vin(v);

        vin << 0 << 1 << 2 << 3 << 4;

        return 0;
    }

QtのQListなどは同様の機能をあらかじめ提供している。

std::mapより速い連想配列を使いたい
==================================

std::unordered_map
------------------

std::unordered_mapはキーのソートを行わない分、std::mapよりも高速である。
ただし利用できるのはC++11から。
C++11以前で使用したい場合はBoostのboost::unordered_mapが利用できる。

.. code-block:: cpp
    :linenos:

    #include <string>
    #include <unordered_map>

    int main() {
        std::unordered_map<std::string, int> m;

        m["hello"] = 0;
        m["world"] = 1;

        return 0;
    }

cpp-btree
---------

Googleが公開しているBツリーというデータ構造ライブラリ。
std::mapと同様、キーによるソートは行われるが
多くの場合でstd::mapより速いと言われている。

cpp-btree

  https://code.google.com/p/cpp-btree/

.. code-block:: cpp

    #include <btree_map.h>

    int main() {
        btree::btree_map<std::string, int> m;

        m["hello"] = 0;
        m["world"] = 1;

        return 0;
    }

構造体をハッシュテーブルのキーにしたい
======================================

文字列をキーにすることは標準で可能なので
構造体のバイト列をstd::stringにしてしまうのが一番楽。

.. code-block:: cpp
    :linenos:

    #include <string>
    #include <type_traits>
    #include <unordered_map>

    struct Key {
        int a;
        int b;
    };

    template <typename T>
    struct Hasher {
        size_t operator()(const T& t) {
            // バイト列に正しく変換できるのはPOD型のみ
            static_assert(std::is_pod<T>::value, "should be POD struct.");

            const char* ptr = reinterpret_cast<const char*>(&t);
            size_t size = sizeof(t);

            return std::hash(std::string(ptr, size));
        }
    };

    int main() {
        std::unordered_map<Key, int, Hasher<Key>> m;
        Key key = {10, 20};

        m[key] = 30;

        return 0;
    }
