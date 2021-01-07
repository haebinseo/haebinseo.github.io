---
layout: post
title: "Python 기초 복습 - print function"
date: "2021-01-07 23:26:00"
author: Haebin Seo
categories: python
tags: python print
---
# print function
- Separator Option
  ```python
  # default sep: ' '
  print('010', '1234', '5678', sep='-')
  # result: 010-1234-5678
  ```

- End Option
  ```python
  # default end: '\n'
  print('Welcome to', end='  ')
  print('IT News', end='  ')
  print('Web Site')
  # result: Welcome to  IT News  Web Site
  ```

- File Option
  ```python
  # default end: 'sys.stdout'
  print('Learn Python', file='~~~')
  ```

- Python Format
  ```python
  # string modulo operator (%). Old way
  # The general syntax for a format placeholder is %[flags][width][.precision]type
  print('Art: %5d, Price per Unit: %8.2f' % (453, 59.058))

  # str.format() method
  print('Art: {0:5d}, Price per Unit: {1:8.2f}'.format(453, 59.058)) # 이 경우 index 생략도 가능
  print('Art: {a:5d}, Price per Unit: {p:8.2f}'.format(a=453, p=59.058))

  # Formatted String Literals
  price = 11.23
  print(f'Price in Euro: {price}') # Price in Euro: 11.23
  print(f'Price in Swiss Franks: {price * 1.086:5.2f}') # Price in Swiss Franks: 12.20
  for article in ["bread", "butter", "tea"]:
    print(f"{article:>10}:")
  #      bread:
  #     butter:
  #        tea:
  ```

  str.format() method 활용
  ```python
  # 문자열일 때 Conversion 's'는 생략가능
  # >10 => 문자열을 지정된 길이로 만든 뒤 오른쪽 정렬, 남은 공간 공백. 왼쪽 정렬은 반대 부등호
  '{:>10}'.format('nice') # '      nice'

  # 남은 공간 문자열 채우기
  '{:_>10}'.format('nice') # '______nice'

  # 가운데 정렬
  '{:^10}'.format('nice') # '   nice   '

  # type 별 .n 의미 차이: string -> n자만, float -> 소숫점 이하를 n자리 까지. width는 min-width에 가까움
  '{:.2}'.format('nice') # 'ni'
  '{:10.2}'.format('nice') # 'ni        '
  '{:1.18f}'.format(3.141592653589793) # '3.141592653589793116'
  '{:06.2f}'.format(3.141592653589793) # '003.14'
  ```

  cf) formatting을 위한 string methods
  ```python
  # S.center(width[, fillchar]) -> str
  s = 'Python'
  s.center(10) # '  Python  '
  s.center(10,"*") # '**Python**'

  # S.ljust(width[, fillchar]) -> str
  s = 'Training'
  s.ljust(12) # 'Training    '

  # S.rjust(width[, fillchar]) -> str
  s = 'Programming'
  s.rjust(15) # '    Programming'

  # S.zfill(width) -> str
  # The string S is never truncated
  account_number = '43447879'
  account_number.zfill(12) # '000043447879'
  ```

- 참조
  - 바이트 코드: <https://namu.wiki/w/%EB%B0%94%EC%9D%B4%ED%8A%B8%EC%BD%94%EB%93%9C>