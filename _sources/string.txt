文字列
******

.. contents::
    :depth: 2

文字列を数値に変換したい
========================

標準ライブラリ利用
------------------

.. code-block:: cpp
    :linenos:

    #include <cstdlb>

    int main() {
        const char* s = "10";

        // 浮動小数点型に変換するならstd::atof()
        int v = std::atoi(s);
    }

Boost利用
---------

.. code-block:: cpp
    :linenos:

    #include <boost/lexical_cast.hpp>

    int main() {
        const char* s = "10";

        int v = boost::lexical_cast<int>(s);
    }

数値を文字列に変換したい
========================

C++11
-----

.. code-block:: cpp
    :linenos:

    #include <string>

    int main() {
        int v = 10;

        const std::string& s = std::to_string(v);
    }

C++11以前
---------

.. code-block:: cpp
    :linenos:

    #include <sstream>
    #include <string>

    int main() {
        int v = 10;

        std::ostringstream strout;

        strout << v;

        // s == "10"
        const std::string& s = strout.str();

        return 0;
    }

文字コードを変換したい
======================

標準ライブラリではできないので、Boostを使用する。

依存関係

* ICU

リンク

* -lboost_locale

.. code-block:: cpp
    :linenos:

    #include <boost/locale/encoding.hpp>

    namespace conv = boost::locale::conv;

    int main() {
        std::string sjis = "...";

        const std::string& utf = conv::to_utf<char>(sjis, "Shift_JIS");

        const std::string& euc = conv::from_utf(utf, "EUC-JP");
    }

UTF <-> その他間での変換しかできないので
ある文字コードから別のある文字コードに変換する時は
一旦UTF-8を経由する必要がある。
