# AIFFEL Campus Online 5th Code Peer Review
- 코더 : 윤상현
- 리뷰어 : 박혜원


# PRT(PeerReviewTemplate)
각 항목을 스스로 확인하고 토의하여 작성한 코드에 적용합니다.

- [△] 코드가 정상적으로 동작하고 주어진 문제를 해결했나요?
  >  SentencePiece를 이용하여 모델을 만들기까지의 과정이 정상적으로 진행되었는가? [O] 

    코퍼스 분석, 전처리, SentencePiece 적용, 토크나이저 구현 및 동작이 빠짐없이 진행되었는가?[O] 

    SentencePiece를 통해 만든 Tokenizer가 자연어처리 모델과 결합하여 동작하는가? [O] 

    SentencePiece 토크나이저가 적용된 Text Classifier 모델이 정상적으로 수렴하여 80% 이상의 test accuracy가 확인되었다.
    result_spm ouput 이 없음. 
    [X]

    SentencePiece의 성능을 다각도로 비교분석하였는가? [△] 

    SentencePiece 토크나이저를 활용했을 때의 성능을 다른 토크나이저 혹은 SentencePiece의 다른 옵션의 경우와 비교하여 분석을 체계적으로 진행하였다. -> Mecab [O] 

- [O] 주석을 보고 작성자의 코드가 이해되었나요?
  > 주석은 없었으나, 이해하는데 문제 없었음. 
- [X] 코드가 에러를 유발할 가능성이 없나요?
  > 아래 정정할 부분 기재함. 
- [O] 코드 작성자가 코드를 제대로 이해하고 작성했나요?
- [O] 코드가 간결한가요?


# def Text_ClassifierModel(model_prefix): 부분 수정 제안 
model이라는 변수가 Text_ClassifierModel 함수 내에서만 정의되고 사용되기 때문에 발생한 에러입니다. 따라서 아래와 같이 수정하면 result 를 확인할 수 있으실 것입니다. 


```python
from sklearn.model_selection import train_test_split
from tensorflow.keras.layers import Dense, LSTM, Embedding
from keras.models import Sequential
from keras.callbacks import EarlyStopping

def Text_ClassifierModel(model_prefix):
    tensor, word_index, index_word = sp_tokenize(s, df['document'])
    x_train, x_val, y_train, y_val = train_test_split(tensor, df['label'], test_size=0.2)
    x_train, x_test, y_train, y_test = train_test_split(x_train, y_train, test_size=0.2)
    # 6 : 2 : 2
    # train : val : test

    word_vector_dim = 32

    model = Sequential()
    model.add(Embedding(vocab_size, word_vector_dim))
    model.add(LSTM(64))
    model.add(Dense(1, activation='sigmoid'))

    model.summary()

    model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])

    es = EarlyStopping(monitor='val_loss', mode='min', verbose=1, patience=4)

    epochs=20
    batch_size=64

    history = model.fit(
        x_train, y_train, epochs=epochs, batch_size=batch_size,
        validation_data=(x_val,y_val), callbacks=[es], verbose=1)

    return model

model_prefix = ...  # define your model prefix
model = Text_ClassifierModel(model_prefix)

result_spm = model.evaluate(x_test, y_test, verbose=0)

```


