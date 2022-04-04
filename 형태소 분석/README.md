# Kkma : Kind Korean Morpheme Analyzer

# 1. KoNLPy - Kkma

### KoNLPy
- '코엔엘파이'는 한국어 정보처리를 위한 파이썬 패키지

### KoNLPy 형태소 분석기
- Hannanum(한나눔) : KAIST Semantic Web Research Center 개발(http://semanticweb.kaist.ac.kr/hannanum/)
- Kkma(꼬꼬마) : 서울대학교 IDS(Intelligent Data Systems) 연구실 개발(http://kkma.snu.ac.kr/)
- Komoran(코모란) : Shineware에서 개발(https://github.com/shin285/KOMORAN)
- Mecab(메카브) : 일본어용 형태소 분석기를 한국어를 사용할 수 있도록 수정(https://bitbucket.org/eunjeon/mecab-ko)
- OpenKoreanText(Okt): 오픈 소스 한국어 분석기, 과거 트위터(Twitter) 형태소 분석기(https://github.com/open-korean-text/open-korean-text)

### Kkma(Kind Korean Morpheme Analyzer)
- 꼬꼬마 프로젝트는 서울대학교 IDS (Intelligent Data Systems) 연구실에서 자연어 처리를 하기 위한 다양한 모듈 및 자료를 구축하기 위한 과제
- 꼬꼬마 한글 형태소 분석기는 Java 라이브러리로써 jar 파일 형태로 개발되었으나, 파이썬에서도 쉽게 사용할 수 있음
- 다른 형태소 분석기와 동일하게 Text의 형태소를 분석하는데 쓰임
- version : kkma-2.1.jar

# 2. Kkma Algorithm

* 알고리즘 참고(http://kkma.snu.ac.kr/)

# 3. 형태소 분석

### 형태소(Morpheme)
- 형태소는 '뜻을 가진 가장 작은 말의 단위'
- '책가방' => '책', '가방'
- 명사, 대명사, 수사, 조사, 동사, 형용사, 관형사, 부사, 감탄사

## 3.1 사용 데이터

* 네이버 영화 리뷰 데이터(ratings.txt)
* Naver sentiment movie corpus v1.0
* 출처 : https://github.com/e9t/nsmc

![image](https://user-images.githubusercontent.com/87981867/161542667-9467b1a9-e916-41b3-9fe6-d8a8716450c1.png)

## 3.2 형태소 단위로 Tokenizing

* Tokenizing : 텍스트 정보를 단위별로 나누는 것
* KoNLPy의 분석기는 Kkma, Okt, Komoran, Hannanum, Mecab
* morphs() : 텍스트를 형태소 단위로 나눔
* nouns() : 텍스트에서 명사만 추출
* pos() : 텍스트를 형태소 단위로 나눈 뒤, 형태소와 품사 정보를 리스트화

<pre><code class="python">
# KoNLPy 설치
!pip install konlpy
</code></pre>

<pre><code class="python">
# 실험 환경
python 3.7
konlpy 0.6.0
</code></pre>

<pre><code class="python">
from konlpy.tag import Kkma
kkm = Kkma()
</code></pre>

<pre><code class="python">
# 예시문
review_data['document'][15]
</code></pre>

<pre><code class="python">
평점 왜 낮아? 긴장감 스릴감 진짜 최고인데 진짜 전장에서 느끼는 공포를 생생하게 전해준다.
</code></pre>

<pre><code class="python">
# pos() 함수로 형태소 분석 태깅
kkm_pos = kkm.pos(review_data['document'][15])
kkm_pos
</code></pre>

<pre><code class="python">
[('평점', 'NNG'),   보통 명사
 ('왜', 'MAG'),     일반 부사
 ('낮', 'VA'),      형용사
 ('아', 'ECD'),     의존적 연결 어미
 ('?', 'SF'),       마침표물음표,느낌표
 ('긴장감', 'NNG'), 보통 명사
 ('스릴', 'NNG'),   보통 명사 => 한 단어
 ('감', 'NNG'),     보통 명사 => 한 단어
 ('진짜', 'NNG'),   보통 명사 => 문맥상 부사
 ('최고', 'NNG'),   보통 명사
 ('이', 'VCP'),     긍정 지정사, 서술격 조사 '이다'
 ('ㄴ데', 'ECE'),   대등 연결 어미
 ('진짜', 'MAG'),   일반 부사 => 문맥상 명사
 ('전장', 'NNG'),   보통 명사
 ('에서', 'JKM'),   부사격 조사
 ('느끼', 'VV'),    동사
 ('는', 'ETD'),     관형형 전성 어미
 ('공포', 'NNG'),   보통 명사
 ('를', 'JKO'),     목적격 조사
 ('생생', 'XR'),    어근
 ('하', 'XSA'),     형용사 파생 접미사
 ('게', 'ECD'),     의존적 연결 어미
 ('전하', 'VV'),    동사
 ('어', 'ECS'),     보조적 연결 어미
 ('주', 'VXV'),     보조 동사
 ('ㄴ다', 'EFN'),   평서형 종결 어미
 ('.', 'SF')]       마침표물음표,느낌표
</code></pre>

<pre><code class="python">
# morphs() 함수로 형태소 분석
kkm_mor = kkm.morphs(review_data['document'][15])
kkm_mor
</code></pre>

<pre><code class="python">
['평점',
 '왜',
 '낮',
 '아',
 '?',
 '긴장감',
 '스릴',
 '감',
 '진짜',
 '최고',
 '이',
 'ㄴ데',
 '진짜',
 '전장',
 '에서',
 '느끼',
 '는',
 '공포',
 '를',
 '생생',
 '하',
 '게',
 '전하',
 '어',
 '주',
 'ㄴ다',
 '.']
</code></pre>

* 한글 형태소의 품사를 '체언, 용언, 관형사, 부사, 감탄사, 조사, 어미, 접사, 어근, 부호, 한글 이외'와 같이 나누고 각 세부 품사를 구분
* 품사 태깅 결과는 품사 태그표(http://kkma.snu.ac.kr/documents/index.jsp?doc=postag) 참고
* 품사를 구체적으로 나누어 태깅
* 일부 오류가 있으나, 구체적으로 품사 구분 

<pre><code class="python">
# nouns() 함수로 명사 분석
kkm_nouns = kkm.nouns(review_data['document'][15])
kkm_nouns
</code></pre>

<pre><code class="python">
['평점', '긴장감', '스릴', '스릴감', '감', '진짜', '최고', '전장', '공포']
</code></pre>

* 위와 마찬가지로 '스릴감'에 대해서 명사 구분 오류
* 대체로 명사를 잘 구분

<pre><code class="python">
sentence = "i love you"
kkm_eng = kkm.pos(sentence)
print(kkm_eng)
</code></pre>

<pre><code class="python">
[('i', 'OL'), ('love', 'OL'), ('you', 'OL')]
</code></pre>

* 영어 같은 경우 OL (외국어)로 판단하여 태깅

## 3.3 형태소 분석 소요 시간 비교

<pre><code class="python">
100%|██████████| 1000/1000 [00:40<00:00, 24.50it/s]
100%|██████████| 1000/1000 [00:01<00:00, 557.98it/s]
100%|██████████| 1000/1000 [00:19<00:00, 51.96it/s]
100%|██████████| 1000/1000 [00:11<00:00, 88.26it/s]
</code></pre>

![image](https://user-images.githubusercontent.com/87981867/161542757-a2184fe9-0586-40b1-8057-e64bf89125a8.png)

# 4. 결론

## 4.1 kkma 장단점

### 장점
- 띄어쓰기를 잘 판단
- 문장에 대한 품사 태깅을 구체적으로 해주어 정확성이 높음

### 단점
- 품사 태깅시 다른 형태소 분석기에 비해 시간이 오래 걸림
- 문맥에 따른 품사 구분 능력이 다소 떨어짐
- 실제로 kkma 보다는 Okt, MeCab 등을 활용

## 4.2 활용성

* 강재지침서 체크리스트 대상으로 형태소 분석 비교(Kkma, Okt, Mecab)

<pre><code class="python">
강재지침서 총칙에 명시된 강재지침서의 목적은 ?
Kkma :  [('강재', 'NNG'), ('지침서', 'NNG'), ('총칙', 'NNG'), ('에', 'JKM'), ('명시', 'NNG'), ('되', 'XSV'), ('ㄴ', 'ETD'), ('강재', 'NNG'), ('지침서', 'NNG'), ('의', 'JKG'), ('목적', 'NNG'), ('은', 'JX'), ('?', 'SF')]
Okt :  [('강재', 'Noun'), ('지침', 'Noun'), ('서', 'Josa'), ('총칙', 'Noun'), ('에', 'Josa'), ('명시', 'Noun'), ('된', 'Verb'), ('강재', 'Noun'), ('지침', 'Noun'), ('서', 'Josa'), ('의', 'Noun'), ('목적', 'Noun'), ('은', 'Josa'), ('?', 'Punctuation')]
Mecab :  [('강재', 'NNG'), ('지침서', 'NNG'), ('총칙', 'NNG'), ('에', 'JKB'), ('명시', 'NNG'), ('된', 'XSV+ETM'), ('강재', 'NNG'), ('지침서', 'NNG'), ('의', 'JKG'), ('목적', 'NNG'), ('은', 'JX'), ('?', 'SF')]
강재지침서 총칙에 명시된 강재지침서의 적용 대상은 ?
Kkma :  [('강재', 'NNG'), ('지침서', 'NNG'), ('총칙', 'NNG'), ('에', 'JKM'), ('명시', 'NNG'), ('되', 'XSV'), ('ㄴ', 'ETD'), ('강재', 'NNG'), ('지침서', 'NNG'), ('의', 'JKG'), ('적용', 'NNG'), ('대상', 'NNG'), ('은', 'JX'), ('?', 'SF')]
Okt :  [('강재', 'Noun'), ('지침', 'Noun'), ('서', 'Josa'), ('총칙', 'Noun'), ('에', 'Josa'), ('명시', 'Noun'), ('된', 'Verb'), ('강재', 'Noun'), ('지침', 'Noun'), ('서', 'Josa'), ('의', 'Noun'), ('적용', 'Noun'), ('대상', 'Noun'), ('은', 'Josa'), ('?', 'Punctuation')]
Mecab :  [('강재', 'NNG'), ('지침서', 'NNG'), ('총칙', 'NNG'), ('에', 'JKB'), ('명시', 'NNG'), ('된', 'XSV+ETM'), ('강재', 'NNG'), ('지침서', 'NNG'), ('의', 'JKG'), ('적용', 'NNG'), ('대상', 'NNG'), ('은', 'JX'), ('?', 'SF')]
강재지침서를 관리하는 책임을 갖고 있는 사람은 ?
Kkma :  [('강재', 'NNG'), ('지침서', 'NNG'), ('를', 'JKO'), ('관리', 'NNG'), ('하', 'XSV'), ('는', 'ETD'), ('책임', 'NNG'), ('을', 'JKO'), ('갖', 'VV'), ('고', 'ECE'), ('있', 'VXV'), ('는', 'ETD'), ('사람', 'NNG'), ('은', 'JX'), ('?', 'SF')]
Okt :  [('강재', 'Noun'), ('지침', 'Noun'), ('서', 'Josa'), ('를', 'Noun'), ('관리', 'Noun'), ('하는', 'Verb'), ('책임', 'Noun'), ('을', 'Josa'), ('갖고', 'Verb'), ('있는', 'Adjective'), ('사람', 'Noun'), ('은', 'Josa'), ('?', 'Punctuation')]
Mecab :  [('강재', 'NNG'), ('지침서', 'NNG'), ('를', 'JKO'), ('관리', 'NNG'), ('하', 'XSV'), ('는', 'ETM'), ('책임', 'NNG'), ('을', 'JKO'), ('갖', 'VV'), ('고', 'EC'), ('있', 'VX'), ('는', 'ETM'), ('사람', 'NNG'), ('은', 'JX'), ('?', 'SF')]
</code></pre>
