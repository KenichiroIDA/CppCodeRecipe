入出力
******

.. contents::
    :depth: 2

標準出力にメッセージを出力したい
================================

.. code-block:: cpp
    :linenos:

    #include <iostream>

    int main() {
        double pi = 3.14;
        std::cout << "Value of Pi is " << pi << std::endl;

        return 0;
    }

ファイルにメッセージを出力したい
================================

.. code-block:: cpp
    :linenos:

    #include <fstream>

    int main() {
        std::ofstream file("file.txt");

        double pi = 3.14;
        file << "Value of Pi is " << pi << std::endl;

        return 0;
    }

ファイルからテキストを1行ずつ読み込みたい
=========================================

.. code-block:: cpp
    :linenos:

    #include <fstream>
    #include <string>

    int main() {
        std::ifstream file("file.txt");
        std::string line;

        while (std::getline(file, line)) {
            // ...
        }
    }

ファイルからデータを1つずつ読み込みたい
=======================================

1つずつというのはスペースで区切られたテキストそれぞれを指す。

.. code-block:: cpp
    :linenos:

    #include <fstream>
    #include <iterator>
    #include <string>

    typedef std::istream_iterator<std::string> iterator;

    int main() {
        std::ifstream file("file.txt");
        iterator end;

        for (iterator it = iterator(file); it != end; ++it) {
            // vはfile.txtから読み込まれた値
            const std::string& v = *it;
        }
    }

ファイルサイズを取得したい
==========================

.. code-block:: cpp
    :linenos:

    #include <fstream>

    int main() {
        std::ifstream file("file.txt");

        // ファイルの末尾に移動
        file.seekg(0, std::ios_base::end);

        size_t file_size = file.tellg();

        // ファイル位置を先頭に戻す
        file.seekg(0, std::ios_base::beg);

        // ...

        return 0;
    }

メッセージを整形して出力したい
==============================

標準ライブラリ利用
------------------

マニピュレータを使って可能。

.. code-block:: cpp
    :linenos:

    #include <iomanip>
    #include <iostream>

    int main() {
        // 16進数表示
        std::cout << std::hex << 10 << std::endl;

        // 幅指定
        std::cout << std::setw(5) << "Hello World!" << std::endl;
    }

cppformat利用
-------------

標準ライブラリが使いにくいので、cppformatをおすすめする。

*cppformat*

    https://github.com/cppformat/cppformat

.. code-block:: cpp
    :linenos:

    #include <format.h>

    int main() {
        int a = 10;
        int b = 20;
        int c = 30;

        fmt::print("{0} + {1} == {2}", a, b, c);
        fmt::print("%x", a);
    }
