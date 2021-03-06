:tocdepth: 3

******************
비트 함수와 연산자
******************

.. contents::

비트 연산자
===========

비트 연산자(Bitwise operator)는 비트 단위로 연산을 수행하며 산술 연산식에서 이용될 수 있다. 피연산자로 정수 타입이 지정되며, **BIT** 타입은 지정될 수 없다. 연산 결과로 **BIGINT** 타입 정수(64비트 정수)를 반환한다. 이때, 하나 이상의 피연산자가 **NULL** 이면 **NULL** 을 반환한다.

아래는 CUBRID가 지원하는 비트 연산자의 종류에 관한 표이다.

**비트 연산자**

+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| 비트 연산자          | 설명                                                                                                                                           | 조건식         | 리턴 값          |
+======================+================================================================================================================================================+================+==================+
| &                    | 비트 단위로 **AND** 연산을 수행하고, **BIGINT** 정수를 반환한다.                                                                               | 17 & 3         | 1                |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| \|                   | 비트 단위로 **OR** 연산을 수행하고, **BIGINT** 정수를 반환한다.                                                                                | 17 \| 3        | 19               |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| ^                    | 비트 단위로 **XOR**  연산을 수행하고, **BIGINT**  정수를 반환한다.                                                                             | 17 ^ 3         | 18               |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| ~                    | 단항 연산자이며, 피연산자의 비트를 역으로 전환(**INVERT**)하는 보수 연산을 수행하고, **BIGINT** 정수를 반환한다.                               | ~17            | -18              |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| <<                   | 왼쪽 피연산자의 비트를 오른쪽 피연산자만큼 왼쪽으로 이동시키는 연산을 수행하고, **BIGINT** 정수를 반환한다.                                    | 17 << 3        | 136              |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+
| >>                   | 왼쪽 피연산자의 비트를 오른쪽 피연산자만큼 오른쪽으로 이동시키는 연산을 수행하고, **BIGINT** 정수를 반환한다.                                  | 17 >> 3        | 2                |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------+----------------+------------------+

BIT_AND
=======

.. function:: BIT_AND (expr)

    집계 함수로서, *expr* 의 모든 비트에 대해 비트 단위 **AND** 연산을 수행한다. 조건절을 만족하는 행이 없는 경우, NULL 을 반환한다.

    :param expr: 정수 타입의 임의의 연산식이다.
    :rtype: BIGINT

.. code-block:: sql

    CREATE TABLE bit_tbl(id int);
    INSERT INTO bit_tbl VALUES (1), (2), (3), (4), (5);
    SELECT 1&3&5, BIT_AND(id) FROM bit_tbl WHERE id in(1,3,5);

::

         1&3&5           bit_and(id)
    ============================================
             1                     1    

BIT_OR
======

.. function:: BIT_OR (expr)

    집계 함수로서, *expr* 의 모든 비트에 대해 비트 단위 **OR** 연산을 수행한다. 조건절을 만족하는 행이 없는 경우, **NULL** 을 반환한다.

    :param expr: 정수 타입의 임의의 연산식이다.
    :rtype: BIGINT

.. code-block:: sql

    SELECT 1|3|5, BIT_OR(id) FROM bit_tbl WHERE id in(1,3,5);

::

         1|3|5            bit_or(id)
    ============================================
              7                     7
               
BIT_XOR
=======
  
.. function:: BIT_XOR (expr)

    집계 함수로서, *expr* 의 모든 비트에 대해 비트 단위 **XOR** 연산을 수행한다. 조건절을 만족하는 행이 없는 경우, **NULL** 을 반환한다.

    :param expr: 정수 타입의 임의의 연산식이다.
    :rtype: BIGINT

.. code-block:: sql

    SELECT 1^2^3, BIT_XOR(id) FROM bit_tbl WHERE id in(1,3,5);

::

         1^3^5            bit_xor(id)
    ============================================
              7                     7

BIT_COUNT
=========

.. function:: BIT_COUNT (expr)

    *expr*\의 모든 비트 중 1로 설정된 비트의 개수를 반환하는 함수이며, 집계 함수는 아니다.

    :param expr: 정수 타입의 임의의 연산식이다.
    :rtype: BIGINT

.. code-block:: sql

    SELECT BIT_COUNT(id) FROM bit_tbl WHERE id in(1,3,5);

::

       bit_count(id)
    ================
           1
           2
           2
