
<img width="500" alt="result" src="https://github.com/HalfMoon1008/Aiffel/assets/86039672/4465f401-9c24-4905-9c7b-fd993bdd0060">

# AIFFEL Campus Online 5th Code Peer Review Templete
- 코더 : 윤상현
- 리뷰어 :황규빈


# PRT(PeerReviewTemplate) 
각 항목을 스스로 확인하고 토의하여 작성한 코드에 적용합니다.

- [ ] 코드가 정상적으로 동작하고 주어진 문제를 해결했나요?
  > 넵
- [ ] 주석을 보고 작성자의 코드가 이해되었나요?
  > 아직, 배움의 깊이가 깊지 않아 전부 이해하지는 못하였지만, 주석이 잘 적혀 있어서 괜찮을거 같음.
- [ ] 코드가 에러를 유발할 가능성이 없나요?
  >
- [ ] 코드 작성자가 코드를 제대로 이해하고 작성했나요?
  > 이해를 넘어, 데이터들의 조합을 통해 새로운 값을 도출해내는 점이 좋았습니다
  > 상관계수를 활용한 점. 너무 좋았습니다
- [ ] 코드가 간결한가요?
  > 위 항목에 대한 근거 작성 필수
# 새로운 feature 만들기

def feature_processing(df):
    df['total_rooms'] = df['bedrooms'] + df['bathrooms']
    # 전체 방 수 = 침실 + 화장실

    df['grade_condition'] = df['grade'] * df['condition']
    # 등급_상태 = 집의 등급 * 집의 전반적인 상태

    df['sqft_total'] = df['sqft_living'] + df['sqft_lot']
    # 전체 면적
    # 피트 기준
    # 주거 공간 면적 + 부지 공간 면적

    df['sqft_total_size'] = df['sqft_living'] + df['sqft_lot'] + df['sqft_above'] + df['sqft_basement']
    # 전체 사이즈
    # 피트 기준
    # 주거 면적 + 부지 면적 + 지하실 제외(차고 등등 _ 이하 차고) + 지하실 포함

    df['sqft_total15'] = df['sqft_living15'] + df['sqft_lot15'] 
    # 15년 기준 전체 면적
    # 15년 기준 주거 면적 + 15년 기준 부지 면적

    df['is_renovated'] = df['yr_renovated'] - df['yr_built']
    # 재건축 _ 재건축을 안했다면 0, 했다면 지어진지 a년 뒤에 재건축
    # 재건축 년도 - 지어진 년도
    
    df['is_renovated'] = df['is_renovated'].apply(lambda x: 0 if x == 0 else 1)
    # 재건축 했다면 1, 안했다면 0
    
    df['roombybathroom'] = df['bedrooms'] / df['bathrooms']
    # 방마다 있는 화장실 수
    # 침실 / 화장실

    df['sqft_total_by_lot'] = (df['sqft_living'] + df['sqft_above'] + df['sqft_basement'])/df['sqft_lot']
    # 주거 면적 % 부지 면적
    # (주거 공간 + 차고 + 지하실) / 부지 면적

    qcut_count = 10
    # 10(피트)을 기준으로 공간 나누기 & int화
    df['qcut_long'] = pd.qcut(df['long'], qcut_count, labels=range(qcut_count))
    df['qcut_lat'] = pd.qcut(df['lat'], qcut_count, labels=range(qcut_count))
    df['qcut_long'] = df['qcut_long'].astype(int)
    df['qcut_lat'] = df['qcut_lat'].astype(int)

    df['date'] = pd.to_datetime(df['date'])
    df['yearmonth'] = df['date'].dt.year*100 + df['date'].dt.month
    df['date'] = df['date'].astype('int')
    return df
```

