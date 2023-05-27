# Korean-MBTI-Conversation-Dataset
MBTI 정보가 라벨링된 대화 형식의 한국어 데이터셋입니다.

[네이버 MBTI 심리 카페](https://cafe.naver.com/mbticafe) 크롤링 및 ChatGPT를 이용하여 화자의 MBTI가 라벨링된 데이터셋을 제작했습니다.

각 데이터셋에 대한 설명은 아래와 같습니다.

## qna_cleaned.tsv
|Column|설명|
|---|---|
|(index)|pandas dataframe에서 자동 생성된 index|
|id|크롤링 시 사용했던 index|
|article_id|카페 게시글 번호|
|menu_id|카페 게시판 번호(11~18)|
|question|게시글 마지막 4문장, 각 문장은 [SEP] 토큰으로 구분되어 있음.|
|answer|댓글 첫 2문장, 각 문장은 [SEP] 토큰으로 구분되어 있음.|
|q_mbti|게시글 작성자의 mbti, 유추 불가능 시 null|
|a_mbti|댓글 작성자의 mbti, 유추 불가능 시 null|

- MBTI 심리 카페의 각 MBTI별 사랑방 게시판의 게시글을 크롤링하여 제작했습니다.
- 닉네임으로부터 MBTI를 유추할 수 있는 경우 해당 컬럼에 mbti 정보를 넣었습니다.
- 전처리 내용
  - 8자 ~ 512자 사이의 컨텐츠만 살렸습니다.
  - hanspell 라이브러리를 사용하여 맞춤법 검사 및 띄어쓰기 수정했습니다.
  - 대부분의 이모지를 제거했습니다.
  - 세 번 이상 반복되는 구는 두 번까지만 반복되도록 했습니다.(repeat_normalize 사용)

## multiple_qna_cleaned.tsv
- MBTI 심리 카페의 신변 잡기 게시판의 게시글을 크롤링하여 제작했습니다.
- 컬럼 형식은 qna_cleaned.tsv와 동일합니다.
- qna_cleaned.tsv 파일은 한 게시글에 한 댓글만 대응되었는데, multiple_qna_cleaned.tsv에서는 한 게시글에 여러 댓글이 대응되도록 크롤링했습니다.

## instagram_chatgpt_F.jsonl, instagram_chatgpt_T.jsonl
- [AIHUB 주제별 텍스트 일상 대화 데이터](https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=543)의 인스타그램 대화 데이터를 일부 사용하여 OpenAI사의 gpt-3.5-turbo 모델로 데이터셋을 만들었습니다.
- 인스타그램 대화 한 쌍을 먼저 모델에 알려주고, 그 다음 발화에 이어지는 대답을 예측하도록 했습니다.
- 모델의 persona는 다음과 같이 지정했습니다.
  
  ```"당신은 제 친한 친구이며, 저와 메신저로 대화하는 상황입니다. 당신의 MBTI 세 번째 유형은 {MBTI_TF}이며, 전형적인 {MBTI_TF} 유형의 사람처럼 대답합니다. 당신은 제가 하는 말에 {MBTI_TF}처럼 반응하거나, 질문에 대답하시면 됩니다. 반말만 사용하고, 한국어로만 대답하세요."```
- 데이터셋 구조는 다음과 같습니다.
```
[
  {
    "idx": {일련 번호(3부터 시작)},
    "query": {(사람의)발화},
    "response": {(AI의)응답}
  }
  ...
]
```
