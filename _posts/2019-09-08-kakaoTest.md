---
layout: post
title:  "2019 09 카카오 코딩 테스트"
date:   2019-09-08 14:00:00 +0900
categories: jekyll update
comments : true
---

**1번 문제 설명**

데이터 처리 전문가가 되고 싶은 “어피치”는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다. 간단한 예로 “aabbaccc”의 경우 “2a2ba3c”(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, “abcabcdede”와 같은 문자열은 전혀 압축되지 않습니다. “어피치”는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.
예를 들어, “ababcdcdababcdcd”의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 “2ab2cd2ab2cd”로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 “2ababcdcd”로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.
다른 예로, “abcabcdede”와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 “abcabc2de”가 되지만, 3개 단위로 자른다면 “2abcdede”가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.
압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

**제한사항**

* s의 길이는 1 이상 1,000 이하입니다.
* s는 알파벳 소문자로만 이루어져 있습니다.

**입출력 예**

|s|result|
|:--|:--|
|"aabbaccc"|	7|
|"ababcdcdababcdcd"|	9|
|"abcabcdede"	|8|
|"abcabcabcabcdededededede"	|14|
|"xababcdcdababcdcd"	|17|

**입출력 예에 대한 설명**

**입출력 예 #1**

문자열을 1개 단위로 잘라 압축했을 때 가장 짧습니다.

**입출력 예 #2**

문자열을 8개 단위로 잘라 압축했을 때 가장 짧습니다.

**입출력 예 #3**

문자열을 3개 단위로 잘라 압축했을 때 가장 짧습니다.

**입출력 예 #4**

문자열을 2개 단위로 자르면 “abcabcabcabc6de” 가 됩니다. 문자열을 3개 단위로 자르면 “4abcdededededede” 가 됩니다. 문자열을 4개 단위로 자르면 “abcabcabcabc3dede” 가 됩니다. 문자열을 6개 단위로 자를 경우 “2abcabc2dedede”가 되며, 이때의 길이가 14로 가장 짧습니다.

**입출력 예 #5**

문자열은 제일 앞부터 정해진 길이만큼 잘라야 합니다. 따라서 주어진 문자열을 x / ababcdcd / ababcdcd 로 자르는 것은 불가능 합니다. 이 경우 어떻게 문자열을 잘라도 압축되지 않으므로 가장 짧은 길이는 17이 됩니다.

```
function solution(s) {
    var answer = 0;
    let iteration = s.length/2;
    let compressed = [];

    for(var i = 1; i <= iteration; i++){
        let number = [];
        let chars = [];
        let compressing = s;
        chars.push(compressing.slice(0,i));
        number.push(1);
        compressing = compressing.slice(i);
        //console.log('start here',number, chars,compressing, i);
        while(compressing.length >= i){
            if(chars[chars.length-1] == compressing.slice(0,i)){
                number[number.length-1]++;
            }
            else{
                number.push(1);
                chars.push(compressing.slice(0,i));
            }
            compressing = compressing.slice(i);
            //console.log(compressing, number, chars, i)
        }
        if(compressing.length > 0){
            chars[chars.length-1]+=compressing;
        }
        console.log(number, chars);
        let temp = '';
        for(var j = 0; j < number.length; j++){
            if(number[j] == 1){
                temp+=chars[j];
            }
            else{
                temp+=number[j].toString();
                temp+=chars[j];
            }
            //console.log('temp', temp);
        }
        compressed.push(temp);

    }
    console.log(compressed);
    compressed.sort(function(a,b){
        return a.length - b.length;
    })
    answer = compressed[0].length;
    return answer;
}
```

**2번 문제 설명**

카카오에 신입 개발자로 입사한 “콘”은 선배 개발자로부터 개발역량 강화를 위해 다른 개발자가 작성한 소스 코드를 분석하여 문제점을 발견하고 수정하라는 업무 과제를 받았습니다. 소스를 컴파일하여 로그를 보니 대부분 소스 코드 내 작성된 괄호가 개수는 맞지만 짝이 맞지 않은 형태로 작성되어 오류가 나는 것을 알게 되었습니다. 수정해야 할 소스 파일이 너무 많아서 고민하던 “콘”은 소스 코드에 작성된 모든 괄호를 뽑아서 올바른 순서대로 배치된 괄호 문자열을 알려주는 프로그램을 다음과 같이 개발하려고 합니다.

**용어의 정의**

'(' 와 ')' 로만 이루어진 문자열이 있을 경우, '(' 의 개수와 ')' 의 개수가 같다면 이를 **균형잡힌 괄호 문자열**이라고 부릅니다. 그리고 여기에 '('와 ')'의 괄호의 짝도 모두 맞을 경우에는 이를 **올바른 괄호 문자열**이라고 부릅니다. 예를 들어, "(()))("와 같은 문자열은 “균형잡힌 괄호 문자열” 이지만 “올바른 괄호 문자열”은 아닙니다. 반면에 "(())()"와 같은 문자열은 “균형잡힌 괄호 문자열” 이면서 동시에 “올바른 괄호 문자열” 입니다.
'(' 와 ')' 로만 이루어진 문자열 w가 “균형잡힌 괄호 문자열” 이라면 다음과 같은 과정을 통해 “올바른 괄호 문자열”로 변환할 수 있습니다.
1. 입력이 빈 문자열인 경우, 빈 문자열을 반환합니다.
2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다. 단, u는 "균형잡힌 괄호 문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다.
3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다.
  3-1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다.
4. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다.
  4-1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다.
  4-2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다.
  4-3. ')'를 다시 붙입니다.
  4-4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다.
  4-5. 생성된 문자열을 반환합니다.
**“균형잡힌 괄호 문자열”** p가 매개변수로 주어질 때, 주어진 알고리즘을 수행해 **“올바른 괄호 문자열”**로 변환한 결과를 return 하도록 solution 함수를 완성해 주세요.

**매개변수 설명**

* p는 '(' 와 ')' 로만 이루어진 문자열이며 길이는 2 이상 1,000 이하인 짝수입니다.
* 문자열 p를 이루는 '(' 와 ')' 의 개수는 항상 같습니다.
* 만약 p가 이미 “올바른 괄호 문자열”이라면 그대로 return 하면 됩니다.

**입출력 예**

|p	|result|
|:---|:---|
|"(()())()"|	"(()())()"|
|")("|	"()"|
|"()))((()"|	"()(())()"|

**입출력 예에 대한 설명**

**입출력 예 #1** 

이미 “올바른 괄호 문자열” 입니다.

**입출력 예 #2**

* 두 문자열 u, v로 분리합니다.
    * u = ")("
    * v = ""
* u가 “올바른 괄호 문자열”이 아니므로 다음과 같이 새로운 문자열을 만듭니다.
    * v에 대해 1단계부터 재귀적으로 수행하면 빈 문자열이 반환됩니다.
    * u의 앞뒤 문자를 제거하고, 나머지 문자의 괄호 방향을 뒤집으면 ""이 됩니다.
    * 따라서 생성되는 문자열은 "(" + "" + ")" + ""이며, 최종적으로 "()"로 변환됩니다.

**입출력 예 #3**

* 두 문자열 u, v로 분리합니다.
    * u = "()"
    * v = "))((()"
* 문자열 u가 “올바른 괄호 문자열”이므로 그대로 두고, v에 대해 재귀적으로 수행합니다.
* 다시 두 문자열 u, v로 분리합니다.
    * u = "))(("
    * v = "()"
* u가 “올바른 괄호 문자열”이 아니므로 다음과 같이 새로운 문자열을 만듭니다.
    * v에 대해 1단계부터 재귀적으로 수행하면 "()"이 반환됩니다.
    * u의 앞뒤 문자를 제거하고, 나머지 문자의 괄호 방향을 뒤집으면 "()"이 됩니다.
    * 따라서 생성되는 문자열은 "(" + "()" + ")" + "()"이며, 최종적으로 "(())()"를 반환합니다.
* 처음에 그대로 둔 문자열에 반환된 문자열을 이어 붙이면 "()" + "(())()" = "()(())()"가 됩니다.
```
function solution(p) {
    var answer = '';
    var u = '';
    var v = '';
    if(p == ''){
        return answer;
    }
    for(let j = 1; j <= p.length; j++){
        u = p.slice(0,j);
        v = p.slice(j);
        if(balance(u)){
            console.log('uv',u,v,j);
            break;
        }
        else{
            continue;
        }
    }

    if(!isBalanced(u)){
        let temp = u;
        let ghost = '(';
        ghost += this.solution(v);
        ghost += ')';
        u=u.slice(1,-1);
        for(let i = 0; i <= u.length-1; i++){
            if(u[i] == '('){
                u = replaceAt(u,i,')');
            }
            else{
                u = replaceAt(u,i,'(');
            }
        }
        ghost+=u;
        answer = ghost;
        console.log('answer u',answer);
        return answer;
    }
    else{
        answer = u+this.solution(v);
        console.log('answer v', answer);
        return answer;
    }
}
function balance(s){
    let b = [0,0];
    if(s.length%2!=0){
        return false;
    }
    for(let i = 0; i < s.length; i++){
        if(s[i] == '('){
            b[0]++;
        }
        else{
            b[1]++;
        }
    }
    if(b[0] != b[1]){
        return false;
    }
    else {
        return true;
    }
}
function replaceAt(string, index, replace) {
  return string.substring(0, index) + replace + string.substring(index + 1);
}
function isBalanced(s){
    let leftChars = [];
    for(let i = 0; i < s.length;i++){
        if(s[i] == '('){
            leftChars.push(s[i]);
        }
        else{
            if(leftChars===''){
                return false;
            }
            if(s[i] == ')' && leftChars[leftChars.length-1] != '('){
                return false;
            }
            leftChars.pop();
        }
    }
    if(leftChars.length <= 0){
        return true;
    }
    else{
        return false;
    }
}
```

**[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]**

친구들로부터 천재 프로그래머로 불리는 “프로도”는 음악을 하는 친구로부터 자신이 좋아하는 노래 가사에 사용된 단어들 중에 특정 키워드가 몇 개 포함되어 있는지 궁금하니 프로그램으로 개발해 달라는 제안을 받았습니다. 그 제안 사항 중, 키워드는 와일드카드 문자중 하나인 '?'가 포함된 패턴 형태의 문자열을 뜻합니다. 와일드카드 문자인 '?'는 글자 하나를 의미하며, 어떤 문자에도 매치된다고 가정합니다. 예를 들어 "fro??"는 "frodo", "front", "frost" 등에 매치되지만 "frame", "frozen"에는 매치되지 않습니다.
가사에 사용된 모든 단어들이 담긴 배열 words와 찾고자 하는 키워드가 담긴 배열 queries가 주어질 때, 각 키워드 별로 매치된 단어가 몇 개인지 순서대로 배열에 담아 반환하도록 solution 함수를 완성해 주세요.

**가사 단어 제한사항**

* words의 길이(가사 단어의 개수)는 2 이상 100,000 이하입니다.
* 각 가사 단어의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
* 전체 가사 단어 길이의 합은 2 이상 1,000,000 이하입니다.
* 가사에 동일 단어가 여러 번 나올 경우 중복을 제거하고 words에는 하나로만 제공됩니다.
* 각 가사 단어는 오직 알파벳 소문자로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.

**검색 키워드 제한사항**

* queries의 길이(검색 키워드 개수)는 2 이상 100,000 이하입니다.
* 각 검색 키워드의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
* 전체 검색 키워드 길이의 합은 2 이상 1,000,000 이하입니다.
* 검색 키워드는 중복될 수도 있습니다.
* 각 검색 키워드는 오직 알파벳 소문자와 와일드카드 문자인 '?' 로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
* 검색 키워드는 와일드카드 문자인 '?'가 하나 이상 포함돼 있으며, '?'는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.
    * 예를 들어 "??odo", "fro??", "?????"는 가능한 키워드입니다.
    * 반면에 "frodo"('?'가 없음), "fr?do"('?'가 중간에 있음), "?ro??"('?'가 양쪽에 있음)는 불가능한 키워드입니다.

**입출력 예**

|words	|queries	|result|
|:--|:--|:--|
|["frodo", "front", "frost", "frozen", "frame", "kakao"]	|["fro??", "????o", "fr???", "fro???", "pro?"]	|[3, 2, 4, 1, 0]|

**입출력 예에 대한 설명**

* "fro??"는 "frodo", "front", "frost"에 매치되므로 3입니다.
* "????o"는 "frodo", "kakao"에 매치되므로 2입니다.
* "fr???"는 "frodo", "front", "frost", "frame"에 매치되므로 4입니다.
* "fro???"는 "frozen"에 매치되므로 1입니다.
* "pro?"는 매치되는 가사 단어가 없으므로 0 입니다.

```
function solution(words, queries) {
    var answer = [];
    queries.forEach((query)=>{
        answer.push(0);
        let length = query.length;
        let findQ = query.split("?");
        if(query[0] != '?'){
            words.forEach((word)=>{
                if(word.length == length){
                    var regex = new RegExp("^"+findQ[0])
                    if(regex.test(word)){
                        answer[answer.length-1]++;
                    }
                }
            });
        }
        else{
            words.forEach((word)=>{
                if(word.length == length){
                    var regex = new RegExp(findQ[findQ.length-1]+"$")
                    if(regex.test(word)){
                        answer[answer.length-1]++;
                    }
                }
            });
        }
    })

    return answer;
}
```
