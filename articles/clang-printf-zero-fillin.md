---
title: "printfの0埋めと桁揃え【C言語】"
emoji: "©"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["C"]
published: true
---

こんにちは！コスパを愛するエンジニアのコウキと申します。
この記事は自分の備忘録として書いています。
もし何か間違っている点や不明点などありましたらコメントおねがいします！

## 0埋め

例えば、時間で07:05と表示したい場合は「%02d」と記載する。

```C:printf.c
#include <stdio.h>

int main(void) {

    int hour = 7;
    int min = 5;

    printf("現在時刻: %02d:%02d", hour, min);

    return 0;
}
```

```
現在時刻: 07:05
```

「%02d」とすることで、2桁の0埋めが実現できる。
もし4桁の0埋めをしたい場合は「%04d」と記述する。

## 桁揃え

また、0を記述しない場合は半角スペース**右詰め**で桁揃えされる。

```C:spaceRightPrintf.c
#include <stdio.h>

int main(void) {

    printf("%4d", 1);
    printf("%4d", 12);
    printf("%4d", 123);
    printf("%4d", 1234);

    return 0;
}
```

```
   1
  12
 123
1234
```

左詰めをしたい場合は数値の前に**マイナス**を記述する。

```C:spaceLeftPrintf.c
#include <stdio.h>

int main(void) {

    printf("%-4d", 1);
    printf("%-4d", 12);
    printf("%-4d", 123);
    printf("%-4d", 1234);

    return 0;
}
```

```
1   
12  
123 
1234
```