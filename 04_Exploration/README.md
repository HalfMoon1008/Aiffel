
<img width="500" alt="result" src="https://github.com/HalfMoon1008/Aiffel/assets/86039672/4465f401-9c24-4905-9c7b-fd993bdd0060">

# AIFFEL Campus Online 5th Code Peer Review Templete
- 코더 : 윤상현
- 리뷰어 : 홍서이


# PRT(PeerReviewTemplate) 
각 항목을 스스로 확인하고 토의하여 작성한 코드에 적용합니다.

- [O] 코드가 정상적으로 동작하고 주어진 문제를 해결했나요?
  > 어텐션 메커니즘이 포함된 알고리즘을 잘 작성하고 주어진 데이터셋에 대하여 text summarization을 잘 진행하였다.

- [O] 주석을 보고 작성자의 코드가 이해되었나요?
  > 네

  ```python
  # 인코더 설계
  encoder_model = Model(inputs=encoder_inputs, outputs=[encoder_outputs, state_h, state_c])

  # 이전 시점의 상태들을 저장하는 텐서
  decoder_state_input_h = Input(shape=(hidden_size,))
  decoder_state_input_c = Input(shape=(hidden_size,))

  dec_emb2 = dec_emb_layer(decoder_inputs)

  # 문장의 다음 단어를 예측하기 위해서 초기 상태(initial_state)를 이전 시점의 상태로 사용. 이는 뒤의 함수 decode_sequence()에 구현
  # 훈련 과정에서와 달리 LSTM의 리턴하는 은닉 상태와 셀 상태인 state_h와 state_c를 버리지 않음.
  decoder_outputs2, state_h2, state_c2 = decoder_lstm(dec_emb2, initial_state=[decoder_state_input_h, decoder_state_input_c])

  ```

- [O] 코드가 에러를 유발할 가능성이 없나요?
  > 없어 보인다.

- [O] 코드 작성자가 코드를 제대로 이해하고 작성했나요?
  > 제대로 이해하고 작성하였다.

- [O] 코드가 간결한가요?
  > 필요한 코드만 간결하게 주석을 달아 작성하였다.
  ```python
  # 디코더 설계
  decoder_inputs = Input(shape=(None,))

  # 디코더의 임베딩 층
  dec_emb_layer = Embedding(tar_vocab, embedding_dim)
  dec_emb = dec_emb_layer(decoder_inputs)

  # 디코더의 LSTM
  # decoder_lstm = LSTM(hidden_size, return_sequences=True, return_state=True, dropout=0.4, recurrent_dropout=0.2)
  decoder_lstm = LSTM(hidden_size, return_sequences=True, return_state=True, dropout=0.4)
  decoder_outputs, _, _ = decoder_lstm(dec_emb, initial_state=[state_h, state_c])
  ```


# 예시

주석을 달고 코드를 깔끔하게 잘 작성하였다.

 ```python
 # 중복 문장을 제거합니다.
data = data.drop_duplicates()

# 특수 문자를 제거합니다.
data['text'] = data['text'].str.replace('[^\w\s]', '')

# 불용어를 제거합니다.
stopwords = nltk.corpus.stopwords.words('english')
data['text'] = data['text'].str.replace(' '.join(stopwords), '')

# 단어를 소문자로 변환합니다.
data['text'] = data['text'].str.lower()

# 단어를 토큰화합니다.
data['tokens'] = data['text'].str.split()
 ```

