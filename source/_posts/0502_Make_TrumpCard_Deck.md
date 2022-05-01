---
title: "트럼프카드 덱 만들기"
author: "woobin"
date: '2022-05-02'
---

# 0502_Make_TrumpCard_Deck

- card_set 리스트로 모으기

```python
card_set = []
pattern_limit = ["♥ ", "♣ ", "♠ ", "◆ "]
number_limit = ["A", 2, 3, 4, 5, 6, 7, 8, 9, 10, "J", "Q", "K"]

for i in pattern_limit:
    for j in number_limit:
        card_one = str(i) + str(j)
        card_set.append(card_one)

print(card_set)
```

![Untitled](/Images/0502_Make_TrumpCard_Deck/Untitled.png)

- 카드 더미에서 랜덤한 카드 하나 추출하기

```python
print(random.choice(card_set))
```

![Untitled](/Images/0502_Make_TrumpCard_Deck/Untitled%201.png)

- 카드의 문양을 알아야 할 때가 있기 때문에 문양을 추출하는 코드

```python
target = card_set[51]
test = target.split(" ")
print(test)
```

![Untitled](/Images/0502_Make_TrumpCard_Deck/Untitled%202.png)