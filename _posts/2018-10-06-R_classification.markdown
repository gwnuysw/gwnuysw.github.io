---
layout: post
title:  "R 분류"
date:   2018-10-06 11:11:00 +0900
categories: jekyll update
comments : true
---

# 분류 :스팸 필터링

현재 작업 디렉토리와 작업 이미지를 세팅합니다.

```
> setwd("~/Documents/study/studyStuff/dataminning/second/");
> save.image(".RData");
```

## 베이즈 스팸 분류기 개발

### 이메일 본문 다루기

#### 라이브러리, 이메일파일 불러오기

```
> library(tm);
> library(ggplot2);
> spam.path<-"../first/ML_for_Hackers-master/03-Classification/data/spam/"
> spam2.path<-"../first/ML_for_Hackers-master/03-Classification/data/spam_2/"
> easyham.path<-"../first/ML_for_Hackers-master/03-Classification/data/easy_ham"
> easyham2.path<-"../first/ML_for_Hackers-master/03-Classification/data/easy_ham_2"
> hardham.path<-"../first/ML_for_Hackers-master/03-Classification/data/hard_ham"
> hardham2.path<-"../first/ML_for_Hackers-master/03-Classification/data/hard_ham_2/"
```
#### 텍스트 추출
모든 메일 본문의 첫줄은 "언제나" 줄바꿈으로 시작합니다. 우선 이 규칙을 활용해서 메시지 텍스트를 추출하는 함수를 작성합니다.

```
> get.msg<-function(path){
+    con<-file(path, open="rt", encoding="latin1")
+    print("--------------------------------------------------------------------------------Start Con------------------------------------------------------------------------------")
+    print(head(con))
+    text<-readLines(con)
+    print("--------------------------------------------------------------------------------Start text------------------------------------------------------------------------------")
+    print(head(text))
+    msg<-text[seq(which(text=="")[1]+1,length(text),1)]
+    print("--------------------------------------------------------------------------------Start msg------------------------------------------------------------------------------")
+    print(head(msg))
+    close(con)
+    return(paste(msg,collapse="\n"))
+ }

```
con 객체로 파일을 엽니다.

text 객체에 con 이용해 메일을 한줄씩 읽습니다.

> text : [첫째줄, 둘째줄, 셋째줄...]

which(text=="")[1]+1은 무슨 의미일까요 작은 데이터로 실험해본결과 text에서 처음 공백이 나온 그 다음번쨰 줄의 인덱스 번호를 나타낸다는 것을 알았습니다.

![which](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/R_classification/0.png?raw=true)

즉 seq함수를 이용해서 text벡터에서 처음으로 공백이 나오는 줄의 다음부터 마지막까지 msg에 저장합니다.

spam.path디렉토리의 모든 파일들을 읽어서 메일의 본문을 all.spam에 벡터형식으로 저장합니다. 그러나 파일중에 cmds파일은 쓸모없는 파일이므로 제외합니다. paste함수를 이용해서 msg객체의 벡터를 한줄로 만듭니다.
```
spam.docs<-dir(spam.path)
spam.docs<-spam.docs(which(spam.docs!="cmds"))
all.spam<-sapply(spam.docs, function(p) get.msg(paste(spam.path,p,sep="/")))
```
다음 사진은 실행결과중 일부 입니다.

![get.msg](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/R_classification/1.png?raw=true)

![all.spam](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/R_classification/2.png?raw=true)

### 말뭉치 만들어 내기

```
> get.tdm<-function(doc.vec){
+    doc.corpus<-Corpus(VectorSource(doc.vec))
+    control<-list(stopwords=TRUE, removePunctuation=TRUE, removeNumbers=TRUE, minDocFreq=2)
+    doc.dtm<-TermDocumentMatrix(doc.corpus,control)
+    return(doc.dtm)
+ }
```

<a href="https://www.rdocumentation.org/packages/tm/versions/0.7-5/topics/VectorSource">VectorSource</a>함수는 각 벡터 원소를 하나의 문서로 해석합니다.

<a href="https://www.rdocumentation.org/packages/tm/versions/0.7-5/topics/Corpus">Corpus</a> 함수는 문서의 모음(?)으로 만든다고합니다.

get.tdm은 문서의 모음에서 불용어를 제거하고, 구두점도 제거하고, 숫자도 제거하고, 말뭉치에서 한번 이상 나타난 단어들만 용서-문서테이블로 만들어줍니다.

### 분류기 개발

spam.tdm을 표준 R행렬로 변환합니다.
```
> spam.matrix <- as.matrix(spam.tdm)
```
다음은 spam.matrix의 일부입니다.

![spam.matrix](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/R_classification/3.png?raw=true)

모든 문서의 단어 출현수를 더합니다.
```
> spam.counts<-rowSums(spam.matrix)
```

다음은 spam.counts의 일부입니다.

![spam.counts](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/R_classification/4.png?raw=true)

단어열과 출현 빈도수열을 뽑아서 데이터 프레임을 다시 만듭니다.

```
> spam.df<-data.frame(cbind(names(spam.counts),as.numeric(spam.counts)),stringsAsFactors=FALSE)
```
다음은 spam.df의 일부입니다.
![spam.counts](https://github.com/gwnuysw/gwnuysw.github.io/blob/master/_images/R_classification/5.png?raw=true)

spam.df의 각 열에 term, frequency라는 이름을 부여하고 문자형인 frequency열을 숫자형으로 바꿉니다.

```
names(spam.df)<-c("term", "frequency")
spam.df$frequency<-as.numeric(spam.df$frequency)
```
