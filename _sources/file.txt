ファイル・パス操作
******************

.. contents::
    :depth: 2

ファイルが存在するかどうかを調べたい
====================================

標準ライブラリではできないのでBoostを使用する。

.. code-block:: cpp
    :linenos:

    #include <boost/filesystem.hpp>

    int main() {
        if (boost::filesystem::exists("file.txt")) {
            // file.txtが存在する
        }

        return 0;
    }

ファイル名から拡張子を取り出したい
==================================

.. code-block:: cpp
    :linenos:

    #include <string>

    int main() {
        std::string filename = "file.txt";

        // ext == ".txt"
        const std::string& ext = filename.substr(filename.rfind("."));

        return 0;
    }
